# Introduction

## What is LayerCI?

LayerCI is a cloud-hosted DevOps platform that creates a new VM for every code change in seconds instead of minutes.

## What are LayerCI VMs useful for?

Many DevOps tasks can be solved in seconds if you can create a VM quickly:

- Staging environments are ready in seconds because the new VM contains a cached database.
- It's easy to run E2E tests in parallel if you can duplicate the entire test VM.
- Build and push your container images effortlessly with the same version of Docker you use in production.

## How is LayerCI different from a traditional cloud provider?

LayerCI is built as a DevOps platform, and not for hosting production code.

Our VMs use memory snapshotting to work like Docker containers. They're faster and more developer-friendly than a traditional VM.

We can create a VM per commit, because we automatically hibernate them when they're not in use.

Because the configuration looks like a Dockerfile, you don't need to configure a complicated build process either.

You simply write a `Layerfile` (similar to a `Dockerfile`) and we'll use memory snapshots to automatically reuse the work done the last time the pipeline ran.

## Is it hard to get started?

We integrate seamlessly with GitHub and GitLab. Getting a "hello world" VM created only takes about five minutes:

1. Log in to LayerCI with your GitHub account.
2. Install LayerCI onto a relevant source code repository.
3. Create a file named 'Layerfile' anywhere in your repository.

<a class="btn btn-lg btn-success" href="/onboarding">Try an interactive tutorial</a>
