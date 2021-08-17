# EXPOSE WEBSITE

`EXPOSE WEBSITE [location on runner] (path) (rewrite path)`

The `EXPOSE WEBSITE` instruction creates a persistent link to view a webserver running at a specific port in the Layerfile. It's especially useful for sharing changes with non-technical stakeholders or running manual QA/review.

Additionally, the `EXPOSE_WEBSITE_HOST` environment variable is available even before `EXPOSE WEBSITE` if you need to "bake" the path to the exposed website URL.

If the default timeout is not sufficient for your application, use the following to wait until the server is ready:
<img width="863" alt="Screen Shot 2021-08-17 at 9 20 49 AM" src="https://user-images.githubusercontent.com/88003890/129733234-0c2b7b2f-9805-4d09-9170-a29897ab6d23.png">

### Examples

- Use `EXPOSE WEBSITE localhost:80` to expose the local webserver at port 80
- Combine `EXPOSE WEBSITE localhost:80 /api` with `EXPOSE WEBSITE localhost:3000 /` to route all requests that start with /api to port 80 in the runner, and all other requests to port 3000.
