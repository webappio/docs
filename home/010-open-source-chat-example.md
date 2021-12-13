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


# Livechat Example Walkthrough

**Step 1:** Go to the [GitHub repository for the Livechat example](https://github.com/webappio/livechat-example) and fork the repository.

<img src="/static/assets/assets-main/github-repo.png" class="Docs-Image" />

<small>Screenshot of the Livechat Example repo.</small>

<img src="/static/assets/assets-main/forked-repo-no-name.png" class="Docs-Image" />

<small>Screenshot after clicking on the button in GitHub to fork the repository.</small>

<div class="section-spacing"></div>

**Step 2:** Clone the new Livechat Example repository to your local machine.

<img src="/static/assets/assets-main/clone-locally.png" class="Docs-Image" />

<small>Click on the "Code" button to get the URL to clone locally.</small>

<pre>
    <code class="language-html CodeHighlight">
    git clone (INSERT URL HERE)
    </code>
</pre>

<div class="section-spacing"></div>

**Step 3:** Sign up to webapp.io and install webapp.io on your GitHub account, **ensuring that webapp.io has access to the repository you created.**

<img src="/static/assets/assets-main/webapp-signup.png" class="Docs-Image" />

<small>Screenshot of the sign-up page for webapp.io</small>

<img src="/static/assets/assets-main/install-webapp-on-repo.png" class="Docs-Image" />

<small>Screenshot of the installations buttons on webapp.io</small>

<div class="section-spacing"></div>

**Step 4:** Make a change to the project locally, and push your changes to the repository you created.

<pre>
    <code class="language-html CodeHighlight">
    git add .
    git commit -m "making some change to Livechat example"
    git push
    </code>
</pre>

<div class="section-spacing"></div>

**Step 5:** Go to your dashboard on webapp.io to see your run, click on “Details”,“main-layerfile”, then “View website”.

<img src="/static/assets/assets-main/run-page.png" class="Docs-Image" />

<small>Screenshot of the run page with the Livechat Example completed.</small>

<img src="/static/assets/assets-main/detail-page.png" class="Docs-Image" />

<small>Screenshot of the detail page. Click on "main-layerfile" here, followed by "View Website".</small>

<div class="section-spacing"></div>

**Step 6:** Wait for the server to start, you’ll be redirected to a preview environment with the Livechat Example. 

<img src="/static/assets/assets-main/livechat-example.png" class="Docs-Image" />

<small>Screenshot of the Livechat Example in a preview environment.</small>

<div class="section-spacing"></div>

That’s all you need to view the full-stack review environment with webapp.io!
Next, try making another change to one of the views (I.e., in `/services/web/src/views/login/login.js`) and push a new commit to see another preview environment with your changes.
