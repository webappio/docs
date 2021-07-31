## Logging in to Docker

webapp.io creates entire VMs as easily as Dockerfiles, so it's common for our users to use Docker or docker-compose within webapp.io.

Docker hub rate limits requests made by unauthenticated users, so it's imperative to create a docker hub account and log in to it to avoid failing tests.

The simplest way is to combine [SECRET ENV](/docs/layerfile-reference#secret-env) with `RUN`:

1. Add a new secret with key "DOCKER_LOGIN" in the secrets pane (the lock icon to the left)
2. Make the value for that secret be your docker hub login
3. Press the "Save" button to save the new secret.

Change your [Layerfile](/docs/examples/docker) and add the following lines after installing Docker:

```Layerfile
SECRET ENV DOCKER_LOGIN
RUN echo "$DOCKER_LOGIN" | docker login --username (INSERT USERNAME) --password-stdin
```

Full example of Layerfile that installs & runs a docker container, then creates a persistent staging link from it:

```Layerfile
FROM vm/ubuntu:18.04

# To note: Layerfiles create entire VMs, *not* containers!

# install the latest version of Docker, as in the official Docker installation tutorial.
RUN apt-get update && \
    apt-get install apt-transport-https ca-certificates curl software-properties-common && \
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add - && \
    add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable" && \
    apt-get update && \
    apt install docker-ce

SECRET ENV DOCKER_LOGIN
RUN echo "$DOCKER_LOGIN" | docker login --username (INSERT USERNAME) --password-stdin

# copy files from the repository into this staging server
COPY . .

RUN docker build -t image .
RUN docker run -d -p 80:80 image
EXPOSE WEBSITE http://localhost:80
```
