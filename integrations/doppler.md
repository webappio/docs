![Doppler Logo](/docs/resources/doppler_logo.png)

# Doppler

[Doppler](https://www.doppler.com/) is a Universal Secrets Manager, a central source of truth for app config and secrets like API keys, database urls, certs, feature flags, and other environment variables. It works seamlessly from local development to production on every language, stack, and infrastructure.

## Example Layerfile

```
FROM vm/ubuntu:18.04

# Install Doppler
RUN (curl -Ls https://cli.doppler.com/install.sh || wget -qO- https://cli.doppler.com/install.sh) | sh

COPY . .

# Load several Doppler tokens from LayerCI
SECRET ENV DOPPLER_TOKEN_PREVIEW
SECRET ENV DOPPLER_TOKEN_PRODUCTION

# Test Doppler secrets access for both
RUN doppler -t $DOPPLER_TOKEN_PREVIEW run -- printenv | grep DOPPLER # Testing purposes only
RUN doppler -t $DOPPLER_TOKEN_PRODUCTION run -- printenv | grep DOPPLER # Testing purposes only
```

### Setting up Doppler with LayerCI

Information on how to integrate Doppler with your LayerCI pipeline can be found [here](https://docs.doppler.com/docs/layerci). 
