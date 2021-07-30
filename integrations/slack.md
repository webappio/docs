![Slack Logo](/docs/resources/slack_logo.svg)

# Slack

Integrate webapp.io into your slack workspace to receive real time updates on CI jobs.

## Adding webapp.io to your Slack

1. Navigate to your organization's settings page at **Settings > Organization**
2. Under **Integrations** click the **Add webapp.io to Slack** button 
3. Allow **Layer** to be added to your Slack workspace

## Subscribing to Notifications in Slack

Subscribing to repository notifications in a public Slack channel will notify the channel of new CI jobs for the repository ran on Layer.
In real time the message will update on the status of all Layerfile runners for the job:

**Running**

![Slack App LayerFile Running](/docs/resources/slack-app-running.png)

**Error**

![Slack App LayerFile Error](/docs/resources/slack-app-error.png)

**Success**

![Slack App LayerFile Success](/docs/resources/slack-app-success.png)

To subscribe to notifications enter `/Layer subscribe [repository owner]/[repository name]` in the public Slack channel of your choice.

To unsubscribe to notifications enter `/Layer unsubscribe [repository owner]/[repository name]`.

