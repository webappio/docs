# WAIT

`WAIT [layerfile paths...]`

The `WAIT` instruction allows you to make one step require other steps to succeed before running.

It's especially useful for conditional actions like executing notifications, deployment, and CI/CD.

### Examples

#### Continuous deployment with WAIT
```Layerfile
# at deploy/Layerfile
FROM vm/ubuntu:18.04

# Wait for the layerfiles at /unit-tests/Layerfile and /acceptance-tests/Layerfile
WAIT /unit-tests /acceptance-tests

RUN ./notify-slack.sh
RUN ./deploy.sh
```


#### Conditional deployment with WAIT and BUTTON
```Layerfile
# at deploy/Layerfile
FROM vm/ubuntu:18.04

# Wait for the layerfiles at /unit-tests/Layerfile and /acceptance-tests/Layerfile
WAIT /unit-tests /acceptance-tests

RUN ./notify-slack.sh
BUTTON deploy?
RUN ./deploy.sh
```

#### What the job view will look like with WAIT

![Advanced workflow graph example](/docs/resources/layerfile-statuses.png)

- Notice that deploy only occurs after all of the other layerfiles have succeeded.
- [This layerfile is available here](https://github.com/distributed-containers-inc/layer-dag-example/blob/794172ae63a6fd59f46d714fcfbefc0a848f98ef/deploy/Layerfile)