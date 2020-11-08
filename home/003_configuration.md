# Configuration

## Layerfiles

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

