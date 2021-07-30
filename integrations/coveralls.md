![Coveralls Logo](/docs/resources/coveralls_logo.png)

# Coveralls

[Coveralls](https://coveralls.io/) is a hosted service designed to provide code coverage analytics. Some key features include: repository coverage statistics, coverage reports, line-by-line coverage, repository overviews, coverage updates, and GitHub notices. With simple step-by-step instructions and built-in integrations with GitHub, GitLab, and Bitbucket, it is easy to get started. 

## Example Layerfile

```
ENV CI_NAME=layerci \
   CI_BUILD_NUMBER=$JOB_ID \
   CI_BUILD_URL="https://webapp.io/$ORGANIZATION_NAME/commits?query=repo%3A$REPOSITORY_NAME+id%3A$JOB_ID" \
   CI_BRANCH="$GIT_BRANCH" \
   CI_PULL_REQUEST="$PULL_REQUEST_URL"

SECRET ENV COVERALLS_REPO_TOKEN

RUN (the test command)
```

### Setting up Coveralls with webapp.io

More information on how to integrate Coveralls with your webapp.io pipeline can be found [here](https://docs.coveralls.io/supported-ci-services).
