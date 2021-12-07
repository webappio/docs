# Cypress

Use the webapp.io Cypress plugin to view end to end test results and videos for. 
The plugin makes use junit XML test reports to gather information from your runs to generate detailed reports. 
These reports can be viewed on the job details page.

## Getting Started
The Cypress plugin can be installed by adding the Cypress widget to your pull requests in the `Pull Request Builder`
section or installed in the `Plugins` section of the `settings tab.

The Cypress plugin looks for the results and videos of test runs in the top level `cypress` directory on the virtual machine. 

To configure Cypress junit XML reporting the following is added to your `cypress.json`: 
```json
{
  "reporter": "junit",
  "reporterOptions": {
    "mochaFile": "cypress/results/test-output-[hash].xml",
    "toConsole": false
  }
}
```

More information on Cypress test reports can be found [here](https://docs.cypress.io/guides/tooling/reporters).



