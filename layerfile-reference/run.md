# RUN 

`RUN (BACKGROUND|REPEATABLE) [command ...]`

The `RUN` instruction runs the given script, and fails the entire Layerfile if the given command fails.

For example, you might use `RUN echo "the directory is $(pwd)"` to print your current directory.

### Examples

- `RUN echo hello` prints "hello" to the terminal
- `RUN BACKGROUND python3 -m http.server` run `python3 -m http.server` persistently in the background.
- `RUN REPEATABLE docker build -t hello` is a performance optimization, see [tuning performance](/docs/tuning-performance#run-repeatable)

### Environment variables
#### CI

`CI=true`, `IS_CI_MACHINE=true`, `CI_MACHINE=true`, `IN_CI_MACHINE=true`, `IN_CI=true`

These `CI` variables are always `true` in LayerCI.


#### GIT\_TAG

`GIT_TAG=v1.0.0`

`GIT_TAG` is the result of running `git describe --always` in the repository.


#### GIT\_COMMIT

`GIT_COMMIT=111122223333444455556666777788889999aaaa`

`GIT_COMMIT` is the result of running `git rev-parse HEAD in the repository.`


#### GIT\_CLONE\_USER
`GIT_CLONE_USER=x-access-token:<token>`

`GIT_CLONE_URL` is a token which can be used to clone this repository. `git clone https://$GIT_CLONE_USER@github.com/org/repo.git`

#### EXPOSE\_WEBSITE\_URL

`EXPOSE_WEBSITE_URL=(uuid).cidemo.co`

`EXPOSE_WEBSITE_URL` is the URL exposed by `EXPOSE WEBSITE`

It's often used to link a frontend with a backend when running both with `EXPOSE WEBSITE` and `RUN BACKGROUND`

You can even reference this before `EXPOSE WEBSITE` is ever used, but the URL is only live after the run passes.


#### LAYERCI

`LAYERCI=true`

`LAYERCI` is always `true` in LayerCI test runs.


#### LAYERCI\_BRANCH

`LAYERCI_BRANCH=staging`

`LAYERCI_BRANCH` is included if this commit was to a specific branch in the repository.
`LAYERCI_BRANCH` is *not* included if this job is running due to an external pull request.


#### LAYERCI\_JOB\_ID

`LAYERCI_JOB_ID=5`

`LAYERCI_JOB_ID` always exists. It's set to the ID of the current running job.


#### LAYERCI\_PULL\_REQUEST

`LAYERCI_PULL_REQUEST=https://github.com/some/repo/pull_requests/5`

`LAYERCI_PULL_REQUEST` may or may not exist. It's a link to the pull request that triggered this pipeline.


#### LAYERCI\_REPO\_NAME

`LAYERCI_REPO_NAME=somerepo`

`LAYERCI_REPO_NAME` is the name of the repository. If the repository is at github.com/a/b, this would be "b"


#### LAYERCI\_REPO\_OWNER

`LAYERCI_REPO_OWNER=repoowner`

`LAYERCI_REPO_OWNER` is the name of the owner of this repository. If the repository is at github.com/a/b, this would be "a"


#### LAYERCI\_ORG\_NAME

`LAYERCI_ORG_NAME=myorg`

`LAYERCI_ORG_NAME` is the name of the current organization. If the dashboard is at layerci.com/myorg, this would be "myorg"


#### LAYERCI\_RUNNER\_ID

`LAYERCI_RUNNER_ID=main-layerfile`

`LAYERCI_RUNNER_ID` is the id of the current layerfile runner.

In a run with a single Layerfile in the root of the directory this will be `main-layerfile`
