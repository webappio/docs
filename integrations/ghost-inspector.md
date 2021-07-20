![Ghost Inspector Logo](/docs/resources/ghost_inspector_logo.svg)

# Ghost Inspector

[Ghost Inspector](https://ghostinspector.com/) allows you to easily create automated browser tests for your websites and web applications. Ensure everything works and looks the way it should. No coding required. 

Using the `npx` command that comes with Node.js you can run a command using the Ghost Inspector CLI without having to install it first. By default the Ghost Inspector CLI will exit successfully even if a suite or test fails, but you can use the `--errorOnFail` flag to exit unsuccessfully and fail the build.

## Example Layerfile

```
# Expose the Ghost Inspector API key as an environment variable
SECRET ENV GHOST_INSPECTOR_API_KEY

# Run your Ghost Inspector suite
# Replace <suite ID> with the appropriate Ghost Inspector suite ID
RUN npx ghost-inspector suite execute <suite ID> \
  --apiKey=$GHOST_INSPECTOR_API_KEY \
  --startUrl=$EXPOSE_WEBSITE_URL \
  --errorOnFail
```

### Setting up Ghost Inspector with Layer

Your Ghost Inspector API key can be found on the main page of your [Ghost Inspector account](https://app.ghostinspector.com/account). This can then be stored and accessed using Layer's [secrets manager](https://layerci.com/docs/layerfile-reference/secret-env), under the name GHOST_INSPECTOR_API_KEY.

More information on how to integrate Ghost Inspector with your Layer pipeline, including how to configure notifications can be found [here](https://ghostinspector.com/docs/integration/layer-ci/).
