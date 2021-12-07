![Reflect.run Logo](/docs/resources/reflect_logo.png)

# Reflect.run

[Reflect.run](https://reflect.run/) is a no-code automated web testing tool that anyone can use. 

<br />

## Example Layerfile

<pre>
    <code class="language-html CodeHighlight">
        #This is an example webapp.io configuration for React!
        FROM vm/ubuntu:18.04
        
        RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add - && \
            echo "deb https://dl.yarnpkg.com/debian/ stable main" > /etc/apt/sources.list.d/yarn.list && \
            curl -fSsL https://deb.nodesource.com/setup_12.x | bash && \
            apt-get install nodejs yarn jq curl
        
        COPY . .
        RUN yarn install
        RUN BACKGROUND yarn start
        
        EXPOSE WEBSITE http://localhost:3000
        
        SECRET ENV REFLECT_API_KEY
        RUN bash the-shell-script-above.sh
    </code>
</pre>

<br />

### Setting up Reflect.run with webapp.io

Use [SECRET ENV](https://webapp.io/docs/layerfile-reference/secret-environments) to add your Reflect API key, and then commit a script like the following:

<pre>
    <code class="language-html CodeHighlight">
        #!/bin/bash
        REQUEST_BODY="
        {
          \"parallelism\": 1,
          \"overrides\": {
            \"hostnames\": [{
              \"original\": \"app-development.useorigin.com\",
              \"replacement\": \"$EXPOSE_WEBSITE_HOST\"
            }]
          }
        }"
        
        EXECUTION_ID=$(curl --location --silent --show-error --request POST 'https://api.reflect.run/v1/tags/suite/executions' \
            --header "X-API-KEY: $REFLECT_API_KEY" \
            --header 'Content-Type: application/json' \
            --data-raw "$REQUEST_BODY" | jq -r '.executionId'
            )
        
        echo "Running the tests... Execution id: $EXECUTION_ID"
        
        STILL_RUNNING_TESTS=true
        while [ "$STILL_RUNNING_TESTS" = "true" ]; do
            EXECUTION_STATUS=$( \
                 curl --location --silent --show-error --request GET "https://api.reflect.run/v1/executions/$EXECUTION_ID" \
                      --header "X-API-KEY: $REFLECT_API_KEY" \
                      --header 'Content-Type: application/json' \
                 )
            
            TESTS_FAILED=$(echo $EXECUTION_STATUS | jq -c '.tests[] | select(.status | contains("failed"))')
            STILL_RUNNING_TESTS=$(echo $EXECUTION_STATUS | jq -c '.tests[] | select(.status | contains("running") or contains("queued")) | length > 0')    if ! [[ -z "$TESTS_FAILED" ]]; then
               printf "\e[1;31mSome tests has failed.\nReflect Execution ID: $EXECUTION_ID\nFailed tests: $TESTS_FAILED\n\n" >&2
               exit 1
            fi
            sleep 1
        done
    </code>
</pre>

More information on how to integrate Reflect.run with your webapp.io pipeline can be found [here](https://reflect.run/docs/integrations/continuous-integration/). 
