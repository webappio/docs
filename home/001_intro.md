# LayerCI Introduction

LayerCI merges the power of dedicated staging servers with the convenience of traditional CI pipelines.

We listen to your source code repository (e.g., GitHub) and create a new staging environment for every single commit.

### Installation
Layer is very simple to set up. It only takes three steps:

1. Sign up (with buttons to the top right, if not already signed in.)
2. Install Layer onto a relevant source code repository (e.g., [GitHub](https://github.com/apps/layerci/installations/new))
3. Create a file named 'Layerfile' anywhere in your repository

Try this file to get started:
```Layerfile
FROM vm/ubuntu:18.04

RUN echo "hello world!"
```

### Example usecases
1. Add a clickable link to every Jira ticket to run manual QA for every pull request, without needing to pay for the resources for multiple environments per developer.
2. Set up your entire back end in 5s by restoring a memory snapshot, then make 10 copies of the VM and run all of your acceptance tests in parallel on every push.
3. Replace your existing staging servers with ones that will automatically hibernate and build every time you push code, without micromanaging when they run.


## The Layerfile

There are a few ways of thinking about these files named 'Layerfile':

1. Auto-discovered Dockerfiles that build entire virtual machines instead of containers, just as quickly.
2. Define a tree of virtual machines. Each subsequent layer can inherit all the running processes in its parents.
3. Trigger specific actions like build, push, test, and deploy in parallel every time you push new code.


#### A sample

```Layerfile

# Start with an ubuntu base image
FROM vm/ubuntu:18.04

# Copy the file named 'file' from the repository to the destination 'hello'
# in the continuous staging environment.
COPY file hello

# 'cat' is a unix directive that prints the contents of a file.
# You could just as easily run a script that runs tests, deploys, or starts a database.
RUN cat hello
```


#### Layers

Behind the scenes, Layerfiles define a linear series of "layers" that stack on top of each other starting at the top of the Layerfile.
After every 20 seconds, we'll snapshot all the files and processes in the VM and store them for later.
In practice, this means that we'll be able to avoid re-running repetitive steps like database migrations the next time 
that you push code.

Unlike the Docker cache, we monitor all the files that are read by any process at the system level. We'll only re-run a layer if we can't find a previous one which agrees on your commit with the repository's files.

Practically, layers mean that all you have to do is put expensive steps as high up in a Layerfile as possible, and we'll skip as many steps as possible the next time you push code.


## The secret sauce

The reason we can offer such powerful staging environments so affordably is because we've created a hypervisor that effectively turns your CI pipelines into time-traveling lambdas.

When you're not using a manual QA environment, we snapshot its memory state and seamlessly wake it up the next time you request it.

Regular CI pipelines are faster because we know which files have been read to set up which steps, and we reuse the entire state of the runner the last time it ran seamlessly.


## Ready to get started?

[Try our interactive onboarding](/onboarding), it takes less than a minute in total!