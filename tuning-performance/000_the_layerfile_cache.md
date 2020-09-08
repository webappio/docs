# Tuning Performance

## The Layerfile cache

LayerCI as extended & improved [Docker's caching model](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#leverage-build-cache) for use in CI.

Consider the following Layerfile:

```Layerfile
FROM vm/ubuntu:18.04
COPY . .
RUN sleep 20 && cat file1
RUN sleep 20 && cat file2
```

In this case, we'll make snapshots after each line and map which files were read back to the snapshots.
This means:
- if you edit any other file than file1 or file2, this entire Layerfile will be skipped.
- if you edit file1, the last two lines will be rerun (40s)
- if you edit file2, only the last line will be rerun (20s)
- if you edit the layerfile, we'll invalidate the cache at the point of the edit.


### Differences from Docker

There are currently three major differences between Layerfiles and Dockerfiles for use in CI:

1. Layerfiles define VMs, not containers - this means you can run anything (including docker) that you could run on a regular cloud server.
2. `COPY` in LayerCI does not invalidate the cache when it runs, instead the files copied are monitored for read/write starting at that point. This means that `COPY . .` is much more common in Layerfiles than Dockerfiles
3. You can copy files from parent directories (`COPY /file1 .` or `COPY ../.. .`) and inherit from other Layerfiles `FROM ../../other/Layerfile`

### Faster installs: The CACHE directive

Sometimes there are steps which will run repeatedly because their constituent files change often, usually source files.
Consider this Layerfile:

```Layerfile
FROM vm/ubuntu:18.04

RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add - && \\
    echo "deb https://dl.yarnpkg.com/debian/ stable main" > /etc/apt/sources.list.d/yarn.list && \\
    curl -fSsL https://deb.nodesource.com/setup_12.x | bash && \\
    apt-get install nodejs

MEMORY 2G
ENV NODE_OPTIONS=--max-old-space-size=8192

COPY package.json ./
CACHE /usr/local/share/.cache/yarn
RUN npm ci
```

In this case, unless you change `package.json`, the default Layer cache will skip the entire pipeline after every push.

The `CACHE` directive only acts to speed up the `npm ci` step in this case.

Note that `CACHE` will "leak" state across runs, so it might allow one run to break all following ones until someone force-retries without caches.
To avoid this problem, only cache stateless directories (which usually contain "cache" in their paths)

Some other examples:
- /var/cache/apt
- /root/.cache/go-build
- ~/.npm ~/.next/cache ~/.yarn/cache

## SPLIT
### Parallelizing directive

Layer gives an exceedingly useful utility to run tests in parallel - `SPLIT 5` duplicates the entire VM 5 times at the point it executes.
In practice this means that you can run tests in parallel without worrying about race conditions causing flaky tests.

#### Rails: knapsack

See [knapsack pro](https://github.com/KnapsackPro/rails-app-with-knapsack)

1. Install the gem
2. Run `KNAPSACK_GENERATE_REPORT=true bundle exec rspec spec` on your local computer
3. `git add knapsack_rspec_report.json && git commit -m 'knapsack' && git push origin master`

Your Layerfile will look something like this:

```Layerfile
# install ruby, bundle install, etc

COPY . .
SPLIT 5
ENV CI_NODE_TOTAL=$SPLIT_NUM CI_NODE_INDEX=$SPLIT
RUN bundle exec knapsack:rspec
```


#### Go: custom test runner

See [this file](https://github.com/distributed-containers-inc/layer-dag-example/blob/master/service-one/parallel-go-test.sh) for an example parallel test runner for go.

The Layerfile from that example:

```Layerfile
FROM ../base/Layerfile

COPY . .
SPLIT 5
RUN ./parallel-go-test.sh
```

## RUN REPEATABLE
### Restores state from previous runs

Sometimes it's not sufficient to just cache directories (`CACHE`), it'd be best to cache complex state such as running processes or mounted files.

LayerCI provides this powerful but dangerous caching mechanism via `RUN REPEATABLE`. It's particularly useful for complicated declarative cluster state like `docker`, `docker-compose` and `kubectl`.

It's recommended to combine RUN REPEATABLE with [multi-stage builds](https://docs.docker.com/develop/develop-images/multistage-build/) for large performance improvements.

#### RUN REPEATABLE for Docker

```Layerfile
# install docker

COPY . .
RUN REPEATABLE docker build -t myimage
RUN docker run -d -p 8080:8080 myimage
```

In this Layerfile, the docker cache from previous runs will be reused because RUN REPEATABLE uses the cache from *after* the last time this step ran.

If you had three pipelines at 9am, 10am, and 11am, the effective steps run would look like this:

- 9am pipeline: cp (9am files) . && docker build -t myimage
- 10am pipeline: cp -a (9am files) . && docker build -t myimage && cp -a (10am files) . && docker build -t myimage
- 10am pipeline: cp -a (9am files) . && docker build -t myimage && cp -a (10am files) . && docker build -t myimage && cp -a (11am files) . && docker build -t myimage

In particular, docker would see that it had been used multiple times, and would be able to re-use the docker cache from previous invocations to greatly improve build speed.

#### RUN REPEATABLE for docker-compose

```Layerfile
# install docker-compose

COPY . .
RUN REPEATABLE docker-compose up -d --build --remove-orphans --force-recreate
```

In this Layerfile, all of these things are reused from the moment immediately after the previous invocation:
- The docker layer cache (e.g., pulled images)
- Any created networks or volumes


#### RUN REPEATABLE for kubernetes (kubectl, k8s, k3s)

```Layerfile
FROM vm/ubuntu:18.04

# install the latest version of Docker, as in the official Docker installation tutorial.
RUN apt-get update && \\
    apt-get install apt-transport-https ca-certificates curl software-properties-common && \\
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add - && \\
    add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable" && \\
    apt-get update && \\
    apt install docker-ce

# install & start k3s
RUN curl -sfL https://get.k3s.io | sh -
RUN BACKGROUND k3s server --docker

# this script might use helm, kompose, jsonnet, or any other manifest handling logic
RUN REPEATABLE ./build-images-and-manifests.sh && k3s kubectl apply -f dist/manifests --prune

EXPOSE WEBSITE localhost:8000
```

*RUN REPEATABLE gives 50-95% speedups here.*

In this Layerfile, we'd set up a kubernetes cluster for you and then snapshot it after you'd started all of your services.

The next time you push, kubernetes' own declarative logic would figure out which pods to delete/restart given the manifests created.
This means that if you had 20 microservices and only changed one, it'd be the only one that is re-deployed with this Layerfile.
