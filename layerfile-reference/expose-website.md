# EXPOSE WEBSITE

`EXPOSE WEBSITE [location on runner] (path) (rewrite path)`

The `EXPOSE WEBSITE` instruction creates a persistent link to view a webserver running at a specific port in the Layerfile. It's especially useful for sharing changes with non-technical stakeholders or running manual QA/review.

Additionally, the `EXPOSE_WEBSITE_HOST` environment variable is available even before `EXPOSE WEBSITE` if you need to "bake" the path to the exposed website URL.

Furthermore, no need for a flag, use this pattern instead: <img width="721" alt="Screen Shot 2021-08-10 at 1 15 21 PM" src="https://user-images.githubusercontent.com/88003890/128905657-745fca17-00a3-4611-8c15-d6e03b36a74a.png">

### Examples

- Use `EXPOSE WEBSITE localhost:80` to expose the local webserver at port 80
- Combine `EXPOSE WEBSITE localhost:80 /api` with `EXPOSE WEBSITE localhost:3000 /` to route all requests that start with /api to port 80 in the runner, and all other requests to port 3000.


