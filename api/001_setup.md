# Setup
You need an HTTP library installed to use the webapp.io API. See code pane for details.

<language-tabs>


<pre>
    <code class="language-ruby CodeHighlight">
        #in bash terminal
        gem install bundler
        bundle install faraday
        
        #at top of file
        require 'faraday'
        require 'json'
    </code>
</pre>


<pre>
    <code class="language-python CodeHighlight">
        #in bash terminal
        pip install requests
        
        #at top of file
        import requests
    </code>
</pre>

<pre>
    <code class="language-shell CodeHighlight">
        #in bash terminal (ubuntu)
        sudo apt-get update
        sudo apt-get install -y curl ca-certificates
        
        #other distributions - google "how to install curl"
    </code>
</pre>

<pre>
    <code class="language-javascript CodeHighlight">
        //for javascript, we use use fetch()
        //for nodejs, https://www.npmjs.com/package/node-fetch
        //i.e., npm install --save node-fetch
    </code>
</pre>

</language-tabs>

<br />
