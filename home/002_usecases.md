# Use-cases

LayerCI is an all-in-one DevOps platform. We provide three main value propositions to our users:

1. Run e2e tests on every commit.
2. Collaborate more easily with per-PR staging environments.
3. Set up CI/CD to build and deploy to production.

### Installation
You can easily set up Layer in three steps:

- [Sign up](https://layerci.com/login).
- Install Layer onto a relevant source code repository (e.g., [GitHub](https://github.com/apps/layerci/installations/new)).
- Create a file named 'Layerfile' anywhere in your repository.

### Start from our examples
We've built a simple [full-stack chat messaging example](https://github.com/layer-devops/livechat-example/) based on [Slack](https://slack.com). It includes three layerfiles:

1. Run e2e tests on every commit:
  - [/layerfiles/base/Layerfile (shared)](https://github.com/layer-devops/livechat-example/blob/main/layerfiles/base/Layerfile)
  - [/layerfiles/e2e-tests/Layerfile](https://github.com/layer-devops/livechat-example/blob/main/layerfiles/e2e-tests/Layerfile)

2. Per-commit demo environments:
  - [/layerfiles/base/Layerfile (shared)](https://github.com/layer-devops/livechat-example/blob/main/layerfiles/base/Layerfile)
  - [/layerfiles/ephemeral-env/Layerfile](https://github.com/layer-devops/livechat-example/blob/main/layerfiles/ephemeral-env/Layerfile)

3. Build and push Docker images:
  - [/layerfiles/docker-push](https://github.com/layer-devops/livechat-example/blob/main/layerfiles/docker-push/build-and-push.sh)
