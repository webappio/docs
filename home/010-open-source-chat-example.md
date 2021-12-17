<br />
<br />

# Quickstart

To view the power of review environments in action, let’s go through an example with our open-source version of Slack, [Livechat Example](https://github.com/webappio/livechat-example), that uses [Docker Compose](https://docs.docker.com/compose/). For the purpose of this quickstart guide, the codebase is monorepository, so all of the services are within a single folder (/services).

Our Livechat Example contains the following within the `/services` folder:

- `/api` (our api to handle all requests)
- `/cypress` (for running tests)
- `/migrate` (for populating our database)
- `/web` (our React frontend)

Most importantly, in the root directory, we have our **Layerfile** which is a **set of instructions that tells webapp.io how to install, build, and run** the Livechat Example. This Layerfile for our Livechat example is shown below:

<pre>
    <code class="language-html CodeHighlight">
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
    </code>
</pre>


To see the Livechat Example in 90 seconds, click the button below **OR** follow the steps below for a more detailed walkthrough.

<a class="btn btn-lg btn-primary" href="/onboarding/github">View a Preview Environment for the Livechat Example</a>

<div class="section-spacing"></div>