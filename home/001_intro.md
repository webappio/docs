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

## Ready to get started?

[Try our interactive onboarding](/onboarding), it takes less than a minute in total!