## Layerfiles can set up a migrated database in 5 seconds

It's common to use Layer to run QA processes against a full stack including open-source databases.

Consider the following Layerfile:

```Layerfile
FROM vm/ubuntu:18.04

# install the latest version of Docker, as in the official Docker installation tutorial.
RUN apt-get update && \\
    apt-get install apt-transport-https ca-certificates curl software-properties-common && \\
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add - && \\
    add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable" && \\
    apt-get update && \\
    apt install docker-ce python3 python3-pip awscli

# install docker compose (easily starts required docker containers)
RUN curl -L "https://github.com/docker/compose/releases/download/1.26.2/docker-compose-$(uname -s)-$(uname -m)" \
    -o /usr/local/bin/docker-compose

# copy files from the repository into this staging server
COPY . .

# start everything - RUN REPEATABLE is a performance improvement that restores the cache from the last time this step ran.
RUN REPEATABLE compose up -d --build --force-recreate --remove-orphans db redis
# run migrations
RUN docker-compose run web python3 manage.py migrate
# download anonymized prod data dump
SECRET ENV AWS_ACCESS_KEY_ID AWS_SECRET_ACCESS_KEY AWS_DEFAULT_REGION
RUN aws s3 cp s3://staging_db_dumps/staging.sql /tmp/staging.sql
RUN cat /tmp/staging.sql | docker-compose exec -T db psql
```

There's a lot to unpack here, but there are a few important takeaways from this example:
1. You can install docker & docker-compose and efficiently create containers within a Layerfile
2. RUN REPEATABLE lets you reuse built images & volumes from the last time this pipeline ran
3. Layer will create a snapshot with everything created so that you can avoid re-building, re-creating, and re-migrating database data every time.

As before, other Layerfiles can extend from this one to run e2e tests or create a full-stack demo environment with `EXPOSE WEBSITE`.