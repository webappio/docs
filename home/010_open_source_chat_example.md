# Quickstart

Let's say you building an open-source version of Slack using [Docker Compose](https://docs.docker.com/compose/).

Your codebase is a monorepository, so all of your services are within a single folder.

### Livechat Example

This is exactly what the [Livechat Example](https://github.com/layer-devops/livechat-example) github project contains.

For code review, you'd like it so that every time a developer pushes code, a new copy of the built services is visible on the internet at **https://(branch).demo.yourcompany.com**

### Layer

Creating environments like this is precisely what Layer helps with. 

Our hosted platform lets you efficiently create many copies of running services by re-using work from prior snapshots.

The Layer configuration from the example above might look like the following:

```Layerfile
FROM vm/ubuntu:18.04

# Install docker
RUN apt-get update && \
    apt-get install apt-transport-https ca-certificates curl software-properties-common && \
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add - && \
    add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable" && \
    apt-get update && \
    apt install docker-ce

# Install docker-compose
RUN curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && \
    chmod +x /usr/local/bin/docker-compose

# Copy repository files
COPY / /root

RUN /root/pull-images.sh
RUN REPEATABLE docker-compose build --parallel
RUN BACKGROUND docker-compose up

# EXPOSE WEBSITE creates an internet visible link
EXPOSE WEBSITE localhost:8000
```

To install Layer with the example above, you'd:

1. Authorize Layer with GitHub, GitLab, or BitBucket
2. Commit the file above as a file named `Layerfile` anywhere in your repository
3. Push a commit that contains that file. Layer will create an environment for it and post a link right on your commit.


<a class="btn btn-lg btn-success" href="/onboarding/github">See a live example in 90s</a>


### Beyond preview environments

Layer is well suited to creating per-branch links of webapps, but that's far from the only thing it can do.

Check out the [Advanced Workflows](/docs/advanced-workflows) section to how polyrepositories, inheritance, and more can be set up with Layer.