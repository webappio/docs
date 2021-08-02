# OAuth (logging in with external sites)

OAuth is what lets you log in to a service with an existing Google or Facebook account.

webapp.io customers often need to set a redirect target from their "test app" on an external service to allow logging in to their own accounts.

For this use case, we've created the `layer-oauth-target.cidemo.co` endpoint, and the flow looks like this:

1. User visits `abcd.cidemo.co`
2. User clicks "log in with Google"
3. User is redirected to a Google login page for a test application
4. The "redirect URI" for that login page is "layer-oauth-target.cidemo.co", so the user is sent to `layer-oauth-target.cidemo.co/oauth/login?code=hello`
5. `layer-oauth-target.cidemo.co` reads a cookie to see which cidemo site the user was last on, so the user is redirected back to `abcd.cidemo.co/oauth/login?code=hello`
6. The application can now read the code and log the user in as usual.


### Combining with white-labeled sites (routing)

The same can be done for layer-oauth-target.demo.example.com, in the case where a route with `$branch.demo.example.com` exists.
