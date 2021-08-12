# BUILD ENV

The `BUILD ENV` instruction tells the layerfile to rebuild when a variable changes.

### Examples

- Commonly used with `$SUBDOMAIN` to ensure each branch has the proper value:

```Layerfile
BUILD ENV SUBDOMAIN
RUN echo "HOST=$SUBDOMAIN.mydomain.com" >> .env
RUN docker-compose up -d
```

### Possible values

#### SUBDOMAIN

`SUBDOMAIN=my-branch`

The `SUBDOMAIN` variable is often used to set the `HOST` variable for webservers.

It is a cleaned up version of the `$GIT_BRANCH` variable, acceptable for use in a URL.

- `feat/add-some-dashboard-pages` becomes `add-some-dashboard-pages`

Common use is to set `HOST=$SUBDOMAIN.demo.example.com`

#### DEPLOYMENT_HOST

`DEPLOYMENT_HOST=job-5.demo.example.com`

The `DEPLOYMENT_HOST` variable is set if a deployment exists for your run.

It's often used to tell a webserver where it is being hosted.

If there are multiple deployments, a single one is returned.

#### CI

`CI=true`, `IS_CI_MACHINE=true`, `CI_MACHINE=true`, `IN_CI_MACHINE=true`, `IN_CI=true`

These `CI` variables are always `true` while running a Layerfile.


#### DEBIAN_FRONTEND

`DEBIAN_FRONTEND=noninteractive`

The `DEBIAN_FRONTEND` variable is always set to `noninteractive` in webapp.io.
To change this behavior, use, e.g., `ENV DEBIAN_FRONTEND=readline`


#### GIT\_TAG

`GIT_TAG=v1.0.0`

`GIT_TAG` is the result of running `git describe --always` in the repository.


#### GIT\_COMMIT

`GIT_COMMIT=111122223333444455556666777788889999aaaa`

`GIT_COMMIT` is the result of running `git rev-parse HEAD` in the repository.


#### GIT\_SHORT\_COMMIT

`GIT_COMMIT=111122223333`

`GIT_COMMIT` is the first 12 characters of running `git rev-parse HEAD` in the repository.

#### GIT\_COMMIT\_TITLE

`GIT_COMMIT_TITLE="[improvement] do something"`

#### GIT\_CLONE\_USER
`GIT_CLONE_USER=x-access-token:<token>`

`GIT_CLONE_URL` is a token which can be used to clone this repository. `git clone https://$GIT_CLONE_USER@github.com/org/repo.git`

#### EXPOSE\_WEBSITE\_HOST

`EXPOSE_WEBSITE_HOST=(uuid).cidemo.co`

`EXPOSE_WEBSITE_HOST` is the hostname exposed by `EXPOSE WEBSITE`

It's often used to link a frontend with a backend when running both with `EXPOSE WEBSITE` and `RUN BACKGROUND`

You can even reference this before `EXPOSE WEBSITE` is ever used, but the URL is only live after the run passes.

Note: Unavailable for use by BUILD ENV

#### WEBAPPIO

`WEBAPPIO=true`

`WEBAPPIO` is always `true` when running a Layerfile


#### GIT\_BRANCH

`GIT_BRANCH=staging`

`GIT_BRANCH` is the branch which is checked out in this repository.


#### JOB\_ID

`JOB_ID=5`

`JOB_ID` always exists. It's set to the ID of the current running job.


#### PULL\_REQUEST\_URL

`PULL_REQUEST_URL=https://github.com/some/repo/pull_requests/5`

`PULL_REQUEST_URL` may or may not exist. It's a link to the pull request that triggered this pipeline.


#### REPOSITORY\_NAME

`REPOSITORY_NAME=somerepo`

`REPOSITORY_NAME` is the name of the repository. If the repository is at github.com/a/b, this would be "b"


#### REPOSITORY\_OWNER

`REPOSITORY_OWNER=repoowner`

`REPOSITORY_OWNER` is the name of the owner of this repository. If the repository is at github.com/a/b, this would be "a"


#### ORGANIZATION\_NAME

`ORGANIZATION_NAME=myorg`

`ORGANIZATION_NAME` is the name of the current organization. If the dashboard is at webapp.io/myorg, this would be "myorg"


#### RUNNER\_ID

`RUNNER_ID=main-layerfile`

`RUNNER_ID` is the id of the current layerfile runner.

#### RETRY\_INDEX

`RETRY_INDEX=1`

`RETRY_INDEX` is the current retry for the given runner (initially 1, then when retried once, 2, etc)

#### API\_EXTRA

`API_EXTRA=some data passed from API`

`API_EXTRA` is optional data passed in when a run is started by the API.