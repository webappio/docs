# Why did a lock exist during RUN REPEATABLE?


## For 'docker run' or 'docker-compose up':

The conditions of this error are:


1. Volume mounts are from the destination of a `COPY` directive.
2. Volume mounts are created in the command run by `RUN REPEATABLE` (e.g., `docker-compose up -d`)
3. The containers keep running after `RUN REPEATABLE` completes.

Some common resolutions are: 

1. **Run `docker-compose up -d` in a separate run directive:** Putting `docker-compose up -d` outside of `RUN REPEATABLE` will break condition (2), so a common solution is something like this:


```
    RUN REPEATABLE docker-compose build --parallel
    RUN docker-compose up -d
```


2. **Don’t use volumes:** Remove volume blocks from your `docker-compose.yml` file and don’t run `docker run` with the `-v` or `--volume` flags. Consider the following example:


```
    "docker-compose -f <(sed /volume:/,2d docker-compose.yml) up -d"

For more complicated files, a command like `yq` can be used for a similar purpose.
```


3. **Copy everything to another directory:** Copying the entire directory somewhere else will resolve this issue, but cause the step to never be skipped (as all files are read):


```
    RUN REPEATABLE rsync -a --delete . /tmp/running/ && cd /tmp/running && docker-compose up -d
```



## For 'npm run start' or other persistent servers

Sometimes, web servers (especially node.js versions) within `RUN REPEATABLE` will cause this problem as well. The simplest solution is to start the webserver in a non-repeatable directive or to copy the files before starting it: 

**Copy everything to another directory**: When copying your files to another directory, your Layerfile may contain something like this:


```
    RUN REPEATABLE rsync -a --delete . /tmp/running/ && cd /tmp/running && ( pkill node || true; ) && nohup npm run start&
```


This is not done by default because it breaks the file watching. However, this guarantees that all files are read.
