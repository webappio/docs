# The Layerfile cache

LayerCI has extended & improved [Docker's caching model](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#leverage-build-cache) for use in CI.

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


## Differences from Docker

Here are the major differences between Layerfiles and Dockerfiles for use in CI:

1. Layerfiles define VMs, not containers - this means you can run anything (including docker) that you could run on a regular cloud server.
2. Running processes are snapshotted and reused. If you start & populate a database, that'll be included in the layer so that you don't have to re-run the steps to set up the database for *every* pipeline.
3. `COPY` in LayerCI does not invalidate the cache when it runs, instead the files copied are monitored for read/write starting at that point. This means that `COPY . .` is much more common in Layerfiles than Dockerfiles
4. You can copy files from parent directories (`COPY /file1 .` or `COPY ../.. .`) and inherit from other Layerfiles `FROM ../../other/Layerfile`

## Faster installs: The CACHE directive

Sometimes there are steps which will run repeatedly because their constituent files change often, usually source files.
Consider this Layerfile:

```Layerfile
FROM vm/ubuntu:18.04

RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add - && \\
    echo "deb https://dl.yarnpkg.com/debian/ stable main" > /etc/apt/sources.list.d/yarn.list && \\
    curl -fSsL https://deb.nodesource.com/setup_12.x | bash && \\
    apt-get install nodejs yarn

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
