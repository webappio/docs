# Deployments

[EXPOSE WEBSITE](/docs/layerfile-reference/expose-website) allows you to whitelabel staging servers on your own domain.

For example, `example.com` could route `$branch.demo.example.com` to the latest commit on the branch `$branch` by adding a single DNS record `CNAME *.demo demotarget.webapp.io` 

How to set up deployments: 
 
Using [webapp.io/dashboard](/dashboard), navigate to your organization’s custom domain.
 
<img width="1189" alt="Screen Shot 2021-08-16 at 11 32 46 AM" src="https://user-images.githubusercontent.com/88003890/129590185-b0fd62cd-add0-44f6-820a-7cc9721134a4.png">

Add the specific domain that you want everything to be exposed under. In the example below, we are adding demo.example.com. A CNAME record will be provided. 
 
<img width="1189" alt="Screen Shot 2021-08-16 at 11 38 54 AM" src="https://user-images.githubusercontent.com/88003890/129590444-5d3ab528-19d7-4104-9313-341de3d88132.png">


Add the CNAME record in your DNS hosting provider (ex: Cloudflare, godaddy, etc). Creating a new record can usually be done within the DNS settings. Once this is done, DNS IS SET UP can be found next to the new domain. 
 
<img width="1054" alt="Screen Shot 2021-08-16 at 11 46 37 AM" src="https://user-images.githubusercontent.com/88003890/129591568-cf70b03b-b33d-44db-9ad6-ba97a29e3c6f.png">

Next, navigate to the deployments tab. On the top right, click ‘NEW’ to create a new deployment rule. Fill in the appropriate fields.
 
<img width="676" alt="Screen Shot 2021-08-16 at 12 07 36 PM" src="https://user-images.githubusercontent.com/88003890/129594474-bcbeee57-f325-47a6-9340-3731ce4efca2.png">


The deployment is now listed under ‘RULES’. In the deployments tab, you can see whether a deployment is on, paused, or deleted. When a deployment is deleted, it can be restored either by rerunning the layerfile or by following the RE-RUN LAYERFILE prompt on the error message page shown below.
 
![Error message when snapshot cannot be loaded](/docs/resources/deployments5.png)

### Use-cases for deployments

- Host your staging servers on your own domain to make them easier to find. `staging.demo.example.com` could be bookmarked by a QA person to see the latest commit on the `staging` branch.
- Run your backend and frontend in different Layerfiles and combine them behind one host (`main.demo.example.com` and `main.demo.example.com/api` respectively)
- Allow multiple subdomains to the same Layerfile in case your application does host based routing (e.g., `dashboard.main.demo.example.com`)

By default, [EXPOSE WEBSITE](/docs/layerfile-reference/expose-website) creates staging servers are at `https://(uuid).cidemo.co`, where the uuid is unique for every Layerfile. The deployments page lets you customize this by adding a column to its table and adding a `CNAME` record on a domain you control.

### Subdomains within deployments

Subdomains are preconfigured in webapp.io. Your webserver always sees the host as localhost. If you don’t want that to be the case, please contact support.

For example, say you have a deployment at deployment.demo.webapp.io. If you then go to hello.deployment.demo.webapp.io, it will go to the same deployment. Similarly, if you navigate to greetings.deployment.demo.webapp.io, it will also direct to deployment.demo.webapp.io. This happens by default. 

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


##### Deployments

1. Create a single deployment from $branch.demo.yourdomain.com to the backend repository, leave the branch field empty

2. Create a CNAME record from *.demo to demotarget.webapp.io

3. Push the layerfiles above to a branch, say, "main"

4. Visit main.demo.yourdomain.com - notice that requests to main.demo.yourdomain.com/api/hello go to the backend layerfile, while requests to main.demo.yourdomain.com go to the frontend layerfile (within the backend domain)
