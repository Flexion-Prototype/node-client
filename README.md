This client has been forked from https://github.com/cmsuser1/node-client

There are assumptions with this default client. The client has a built-in workflow that can be changed if desired.
This client requires Node version v10.1.0

# Flexion prototype workflow

## Authenticating/authorizing the client

First, execute the user setup and OAuth application creation documented in [the
app README](https://github.com/Flexion-Prototype/cms-beneficiary-api).

Save the OAuth application's client ID, client secret, and redirect URI to
configure the authentication library in the test client. Note that the redirect
URI used when requesting grants and tokens _must_ match the one configured on
the application _exactly_.

```
npm install
npm start

// Set to the appropriate Flexion prototype instance:
// For local testing: http://localhost:8000/
// In production: https://CHANGEME/
client.setBaseUrl('URL');
// Use the three values saved on the application created above
client.setAuth('CLIENT ID', 'CLIENT AUTH', 'REDIRECT URI');

// Obtain an authorization URL
client.getAuthUrl()
```

Visit the URL returned by `getAuthUrl()` **in a browser authenticated as the ACO
admin user you created**; check the box to indicate consent, and submit the form.

The authorization server will redirect the browser to the redirect URI
specified in the application; copy the URI, including all generated query
parameters.

Then, in the Node client, request a token using the returned authorization code
(will be parsed from the redirect URI):

```
client.fetchToken('<FULL GENERATED REDIRECT URI>')
```

The client is now authenticated and authorized to trigger exports of data for
the ACO ID attached to the granting admin user.

## Downloading exports

```
client.getBulkRequest('<ACO ID>')
```
