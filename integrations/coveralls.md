![Coveralls Logo](/docs/resources/coveralls_logo.png)

# Coveralls

[Coveralls](https://coveralls.io/) is a hosted service designed to provide code coverage analytics. Some key features include: repository coverage statistics, coverage reports, line-by-line coverage, repository overviews, coverage updates, and GitHub notices. With simple step-by-step instructions and built-in integrations with GitHub, GitLab, and Bitbucket, it is easy to get started. 

## Example Layerfile

```
ENV CI_NAME=layerci \
   CI_BUILD_NUMBER=$LAYERCI_JOB_ID \
   CI_BUILD_URL="https://layerci.com/$LAYERCI_ORG_NAME/commits?query=repo%3A$LAYERCI_REPO_NAME+id%3A$LAYERCI_JOB_ID" \
   CI_BRANCH="$LAYERCI_BRANCH" \
   CI_PULL_REQUEST="$LAYERCI_PULL_REQUEST"

SECRET ENV COVERALLS_REPO_TOKEN

RUN (the test command)
```

### Setting up Coveralls with LayerCI

More information on how to integrate Coveralls with your LayerCI pipeline can be found [here](https://docs.coveralls.io/supported-ci-services).
