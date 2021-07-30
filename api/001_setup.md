# Setup
You need an HTTP library installed to use the webapp.io API. See code pane for details.

<language-tabs>

```ruby
#in bash terminal
gem install bundler
bundle install faraday

#at top of file
require 'faraday'
require 'json'
```

```python
#in bash terminal
pip install requests

#at top of file
import requests
```

```shell
#in bash terminal (ubuntu)
sudo apt-get update
sudo apt-get install -y curl ca-certificates

#other distributions - google "how to install curl"
```

```javascript
//for javascript, we use use fetch()
//for nodejs, https://www.npmjs.com/package/node-fetch
//i.e., npm install --save node-fetch
```
</language-tabs>
