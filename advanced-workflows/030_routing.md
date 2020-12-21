# Routing

[EXPOSE WEBSITE](/docs/layerfile-reference/expose-website) allows you to whitelabel staging servers on your own domain.

For example, `example.com` could route `$branch.demo.example.com` to the latest commit on the branch `$branch` by adding a single DNS record `CNAME *.demo demotarget.layerci.com` 

### Use-cases for routing

- Host your staging servers on your own domain to make them easier to find. `staging.demo.example.com` could be bookmarked by a QA person to see the latest commit on the `staging` branch.
- Run your backend and frontend in different Layerfiles and combine them behind one host (`main.demo.example.com` and `main.demo.example.com/api` respectively)
- Allow multiple subdomains to the same Layerfile in case your application does host based routing (e.g., `dashboard.main.demo.example.com`)

![View of routing page in main dashboard](/docs/resources/routing.png)


By default, [EXPOSE WEBSITE](/docs/layerfile-reference/expose-website) creates staging servers are at `https://(uuid).cidemo.co`, where the uuid is unique for every Layerfile. The routing page lets you customize this by adding a column to its table and adding a `CNAME` record on a domain you control.

### Two layerfile polyrepo example

- In this example, the backend and frontend are separate repositories, and we want to use the latest version of the frontend whenever the backend is built.

##### (backend repo)/layerfiles/backend/Layerfile

```bash
# backend
FROM vm/ubuntu:18.04

# install the latest version of Docker, as in the official Docker installation tutorial.
RUN apt-get update && \
    apt-get install apt-transport-https ca-certificates curl software-properties-common && \
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add - && \
    add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable" && \
    apt-get update && \
    apt install docker-ce

COPY / .
RUN REPEATABLE docker build -t backend && docker run -d -p 80:80 backend

EXPOSE WEBSITE localhost:80 /api

``` 

##### (backend repo)/layerfiles/frontend/Layerfile

```bash
# backend
FROM vm/ubuntu:18.04

# install the latest version of Docker, as in the official Docker installation tutorial.
RUN apt-get update && \
    apt-get install apt-transport-https ca-certificates curl software-properties-common && \
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add - && \
    add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable" && \
    apt-get update && \
    apt install docker-ce

RUN curl -Lo /usr/local/bin/fast-git-download https://gist.githubusercontent.com/ColinChartier/6bff7cf77adf7d2a8d7d699a5deed707/raw/0b89b3037548ce7e4fb24bea96628014da1bbf05/download && \
    chmod 755 /usr/local/bin/fast-git-download

# download the latest version of the frontend's "master" branch and build and start it.
RUN REPEATABLE fast-git-download frontend-repo-name /frontend origin/master && \
    cd /frontend && \
    docker build -t frontend && docker run -d -p 80:80 frontend

EXPOSE WEBSITE localhost:80

``` 


##### Routes

1. Create a single route from $branch.demo.yourdomain.com to the backend repository, leave the branch field empty

2. Create a CNAME record from *.demo to demotarget.layerci.com

3. Push the layerfiles above to a branch, say, "main"

4. Visit main.demo.yourdomain.com - notice that requests to main.demo.yourdomain.com/api/hello go to the backend layerfile, while requests to main.demo.yourdomain.com go to the frontend layerfile (within the backend domain)