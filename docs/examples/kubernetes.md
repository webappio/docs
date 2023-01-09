---
title: "Kubernetes"
---

```docker Layerfile
FROM vm/ubuntu:18.04

# install the latest version of Docker, as in the official Docker installation tutorial.
RUN apt-get update && \
    apt-get install ca-certificates curl gnupg lsb-release && \
    sudo mkdir -p /etc/apt/keyrings && \
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg && \
    echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" |\
    sudo tee /etc/apt/sources.list.d/docker.list > /dev/null && \
    apt-get update && \
    apt-get install docker-ce docker-ce-cli containerd.io dnsmasq

# install & start k3s
RUN curl -sfL https://get.k3s.io | INSTALL_K3S_VERSION=v1.21.2+k3s1 sh -s - --docker

# copy the root (i.e., repository root) to /root in the runner
COPY / /root

# TODO: log in to docker hub to avoid rate limits
# See https://webapp.io/docs/advanced-workflows#logging-in-to-docker to learn how to log in to docker

RUN REPEATABLE echo "replace this line with your image build command" && docker build -t image build-dir

# systemctl restart k3s is required here because tokens expire when things are hibernated
RUN systemctl restart k3s && echo "replace this line with your kubectl deploy command" && k3s kubectl apply -f deploy.yaml

# k3s comes with an ingress controller listening on port 443 already
# you could disable it and use "kubectl port-forward --address 0.0.0.0 svc/my-service 443:443" otherwise
# (change options of k3s install to  --docker --disable traefik to disable the default ingress controller)
EXPOSE WEBSITE https://localhost:443
```

[Try this example live](/sign-up)
