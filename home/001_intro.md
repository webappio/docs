# Introduction

LayerCI is the newest iteration of hosted CI providers, built with webapps in mind.

The platform is more convenient to use than a traditional Docker-based CI, and just as powerful as provisioning a new VM from scratch for every commit.

## How is LayerCI different from traditional hosted CI?

The big difference is our "layer cache" - it acts like `docker build` except it's adapted for CI workloads. Instead of using a `Dockerfile` you write one or more `Layerfile`s.

It's easier to explain with an example: You'd like to start a fresh MySQL database, migrate it, and run end-to-end tests for every proposed change.

In LayerCI, all you'd have to do is put this file in the root of your repository:
```
FROM vm/ubuntu:18.04

RUN apt-get update && apt-get install mysql
COPY . .

# this step might take minutes!
RUN python setup.py migrate

# where the actual tests only take seconds
RUN ./e2etests.sh
```

Every time a developer pushed code to your repository, we'd be able to skip the `setup.py migrate` step because the layer cache would contain a memory snapshot.

Docker would not let you keep the database running between the build steps.

## How does LayerCI provide ephemeral environments?

Because our layer cache contains memory snapshots, we can provide an [ephemeral environment](https://layerci.com/blog/what-is-an-ephemeral-environment/) for every branch.

When you visit `my-branch.demo.example.com`, our [Deployments](/docs/advanced-workflows/deployments) feature will wake up an appropriate environment, and then send your requests to the server within.

For a concrete example, a repository using Flask might contain this Layerfile to configure an ephemeral environment:

```
FROM vm/ubuntu:18.04

RUN apt-get update && apt-get install mysql
COPY . .
RUN python setup.py migrate

RUN BACKGROUND FLASK_APP=flask.py python -m flask run --host=0.0.0.0
EXPOSE WEBSITE localhost:5000
```

## How can I evaluate LayerCI?

We integrate with GitHub, GitLab, and BitBucket. All you have to do is follow the relevant prompts in our [interactive onboarding](/onboarding)

You can choose to only give us access to a single repository for evaluation purposes.

Once you've installed LayerCI, you can build a `Layerfile` like the one above and commit it to your repository. We'll automatically run it and post the status back to GitHub/GitLab/BitBucket.


<a class="btn btn-lg btn-success" href="/onboarding">See a live example in 90s</a>

## How expensive is it?

For personal projects, free.

For startups and larger teams, we have two offerings: a flat fee of $5/developer/month or $35/developer/month (when billed annually) to keep incentives aligned.

For enterprise organizations, we are happy to provide a quote upon request. More information on the features of each offering can be found on our [pricing page](https://layerci.com/pricing).

We don't want to charge per build minute because that would incentivize us to slow things down for profit. 

## Next steps
- [View the guided onboarding to actually try our dashboard](/onboarding)
- [Go to the Examples page](/docs/examples) for full configurations of various web frameworks
- [Read more about inheritance](/docs/advanced-workflows/intro) (running E2E tests in parallel with ephemeral environments, for example)
- [Check out a demo GitHub repository](https://github.com/layer-devops/livechat-example)
- [See how other users have configured LayerCI](https://layerci.com/tag/customer-success/)