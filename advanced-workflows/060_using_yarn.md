## Using Yarn

[Yarn](https://yarnpkg.com/) is a popular JavaScript package manager that is often used as an alternative to npm. 

To install Yarn, your Layerfile will include something like this:
```
FROM vm/ubuntu:18.04
# To note: Layerfiles create entire VMs, *not* containers!
RUN curl -fSsL https://deb.nodesource.com/setup_12.x | bash && \
    curl -fSsL https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" > /etc/apt/sources.list.d/yarn.list && \
    apt-get update && \
    apt-get install nodejs yarn
COPY . .
RUN yarn install --frozen-lockfile
RUN BACKGROUND yarn start
EXPOSE WEBSITE http://localhost:3000
```

Information on optimizing and troubleshooting Yarn in LayerCI can be found [here](https://layerci.com/docs/common-problems/what-is-causing-a-yarn-error).
