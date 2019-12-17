## Usage

Import the Octokit constructor.

For direct usage in browsers, download `octokit-rest.min.js` from the [latest release](https://github.com/octokit/rest.js/releases/latest) and import it using a `<script src="octokit-rest.min.js"></script>` tag.

```
const Octokit = require("@octokit/rest");
```

Now instantiate your octokit API. All options are optional, but authentication is strongly encouraged.

```
const octokit = Octokit({
```

You can set `auth` to a personal access token string.

Learn more about [authentication](https://octokit.github.io/rest.js/#authentication).

```
  auth: "secret123",
```

Setting a user agent [is required](https://developer.github.com/v3/#user-agent-required). It defaults to `octokit/rest.js v1.2.3` where `v1.2.3` is the current version of `@octokit/rest`, but you should set it to something that identifies your app or script.

```
  userAgent: 'myApp v1.2.3',
```

API Previews can be enabled globally by setting the `previews` option. They can be set per-request as well.

Learn more about [API Previews](https://octokit.github.io/rest.js/#previews).

```
  previews: ['jean-grey', 'symmetra'],
```

A default time zone can be enabled by setting the `timeZone` option.

```
  timeZone: 'Europe/Amsterdam',
```

In order to use Octokit with GitHub Enterprise, set the `baseUrl` option.

```
  baseUrl: 'https://api.github.com',
```

For custom logging, pass an object with `debug`, `info`, `warn` and `error` methods as the `log` option.

Learn more about [logging](https://octokit.github.io/rest.js/#logging) and [debugging](https://octokit.github.io/rest.js/#debug).

```
  log: {
    debug: () => {},
    info: () => {},
    warn: console.warn,
    error: console.error
  },
```

Custom request options can be passed as `request.*` options. See [`@octokit/request`](https://www.npmjs.com/package/@octokit/request#octokitrequest) options. The same options can be passed to each endpoint request method.

```
  request: {
    agent: undefined,
    fetch: undefined,
    timeout: 0
  }
})
```

Most of GitHub’s REST API endpoints have matching methods. All endpoint methods are asynchronous, in order to use `await` in the code examples, we wrap them into an anonymous async function.

```
(async () => {
```

For example to retrieve a pull request, use [`octokit.pulls.get()`](https://octokit.github.io/rest.js/#octokit-routes-pulls-get). We recommend to use the search above to find the endpoint method you are looking for

```
const { data: pullRequest } = await octokit.pulls.get({
  owner: "octokit",
  repo: "rest.js",
  pull_number: 123
});
```

Some API endpoints support alternative response formats, see [Media types](https://developer.github.com/v3/media/). For example, to [`request the above pull request in a diff format`](https://developer.github.com/v3/media/#diff), pass the `mediaType.format` option.

Learn more about [request formats](https://octokit.github.io/rest.js/#request-formats)

```
const { data: diff } = await octokit.pulls.get({
  owner: "octokit",
  repo: "rest.js",
  pull_number: 123,
  mediaType: {
    format: "diff"
  }
});
```

For the API endpoints that do not have a matching method, such as the [root endpoint](https://developer.github.com/v3/#root-endpoint) or legacy endpoints, you can send custom requests.

Learn more about [custom requests](https://octokit.github.io/rest.js/#custom-requests).

```
const { data: root } = await octokit.request("GET /");
```

You can also register custom endpoint methods, which is particularly useful if you participate in a private beta.

Learn more about [custom endpoint methods](https://octokit.github.io/rest.js/#custom-endpoint-methods).

```
await octokit.registerEndpoints({
  misc: {
    getRoot: {
      method: "GET",
      url: "/"
    }
  }
});
```

Some endpoints return a list which has to be paginated in order to retrieve the complete data set.

Learn more about [pagination](https://octokit.github.io/rest.js/#pagination).

```
  const options = octokit.issues.listForRepo.endpoint.merge({
    owner: 'octokit',
    repo: 'rest.js'
  })
  octokit.paginate(options)
    .then(issues => {
      // issues is an array of all issue objects
    })
})
```

You can add more functionality with plugins. We recommend the retry and throttling plugins.

Learn more about [throttling](https://octokit.github.io/rest.js/#throttling), [automatic retries](https://octokit.github.io/rest.js/#automatic-retries) and building your own [Plugins](https://octokit.github.io/rest.js/#plugins).

```
import plugin as retry from '@octokit/plugin-retry'
import plugin as throttling from '@octokit/plugin-throttling'

const MyOctokit = Octokit.plugin([
  retry,
  throttling
])
```

`Octokit.plugin()` returns a new constructor. The same options can be passed to the constructor. The options are passed on to all plugin functions as the 2nd argument

```
const myOctokit = new MyOctokit({
  auth: "secret123",
  throttle: {
    onRateLimit: (retryAfter, options) => {
      myOctokit.log.warn(
        `Request quota exhausted for request ${options.method} ${options.url}`
      );

      if (options.request.retryCount === 0) {
        // only retries once
        myOctokit.log.info(`Retrying after ${retryAfter} seconds!`);
        return true;
      }
    },
    onAbuseLimit: (retryAfter, options) => {
      // does not retry, only logs a warning
      myOctokit.log.warn(
        `Abuse detected for request ${options.method} ${options.url}`
      );
    }
  },
  retry: {
    doNotRetry: ["429"]
  }
});
```

```
const octokit = require("@octokit/rest");

// Compare: https://developer.github.com/v3/repos/#list-organization-repositories
octokit.repos
  .listForOrg({
    org: "octokit",
    type: "public"
  })
  .then(({ data, headers, status }) => {
    // handle data
  });
```

## Authentication

While authentication is not strictly required from some API endpoints, GitHub encourages to authenticated all requests to GitHub. Unauthenticated requests are subject to much more aggressive rate limiting.

To enable authenticated requests, pass an `auth` option to the Octokit constructor:

```
const clientWithAuth = new Octokit({
  auth: "token secret123"
});
```

The `auth` option can be

1.  A string
    
    The value will be passed as value for the `Authorization` header, see [authentication](https://developer.github.com/v3/#authentication).
    
    ```
    new Octokit({
     auth: "secret123"
    });
    ```
    
    The string can be one of
    
    1.  Personal access tokens  
        You can [create personal access tokens](https://github.com/settings/tokens) in your account’s developer settings or use the "[Create a new authorization](https://octokit.github.io/rest.js/#octokit-routes-oauthAuthorizations-create-authorization)" endpoint.
    2.  OAuth access tokens  
        Retrieved using the [web application flow](https://developer.github.com/apps/building-oauth-apps/authorizing-oauth-apps/#web-application-flow). See also: [octokit/oauth-login-url.js](https://github.com/octokit/oauth-login-url.js).
    3.  GitHub App bearer tokens  
        See [octokit/app.js](https://github.com/octokit/app.js) to retrieve a JSON Web Token (JWT) in order to authenticate as a GitHub App.
    4.  GitHub App installation tokens  
        See [octokit/app.js](https://github.com/octokit/app.js) to retrieve an installation access token in order to authenticate as a GitHub App installation. Also see `4. A function` below for an example.
2.  As object with the properties `username`, `password`, `on2fa`.
    
    `on2fa` is an asynchronous function that must resolve with two-factor authentication code sent to the user.
    
    ```
    new Octokit({
     auth: {
       username: "octocat",
       password: "secret",
       async on2fa() {
         // example: ask the user
         return prompt("Two-factor authentication Code:");
       }
     }
    });
    ```
    
3.  An object with the properties `clientId` and `clientSecret`
    
    OAuth applications can authenticate using their `client_id` and `client_secret` in order to [increase the unauthenticated rate limit](https://developer.github.com/v3/#increasing-the-unauthenticated-rate-limit-for-oauth-applications). Find the id and secret in your GitHub account’s developer setting, or, if the OAuth app belongs to a GitHub organization, the organization’s developer settings.
    
4.  A function
    
    Must resolve with a string which then will be passed as value for the `Authorization` header. The function will be called before each request and can be asynchronous.
    
    ```
    new Octokit({
     auth() {
       return "token secret123";
     }
    });
    ```
    
    This is useful for GitHub apps, as installations need to renew their tokens each hour. Here is an example on how to implement authentication for GitHub Apps
    
    ```
    const { App } = require("@octokit/app");
    const Octokit = require("@octokit/rest");
    
    const app = new App({
     id: process.env.APP_ID,
     privateKey: process.env.PRIVATE_KEY
    });
    const octokit = new Octokit({
     async auth() {
       const installationAccessToken = await app.getInstallationAccessToken({
         installationId: process.env.INSTALLATION_ID
       });
       return `token ${installationAccessToken}`;
     }
    });
    ```
    
    See also: [https://github.com/octokit/app.js#authenticating-as-an-installation](https://github.com/octokit/app.js#authenticating-as-an-installation).
    

## Previews

To enable any of [GitHub’s API Previews](https://developer.github.com/v3/previews/), pass the `previews` option to the GitHub constructor

```
const octokit = new Octokit({
  previews: ["mercy-preview"]
});
```

Previews can also be enabled for a single request by passing the `mediaType.preview` option

```
const {
  data: { topics }
} = await octokit.repos.get({
  owner: "octokit",
  repo: "rest.js",
  mediaType: {
    previews: ["symmetra"]
  }
});
```

## Request formats & aborts

Some API endpoints support alternative response formats, see [Media types](https://developer.github.com/v3/media/).

For example, to request a [pull request as diff format](https://developer.github.com/v3/media/#diff), set the `mediaType.format` option

```
const { data: prDiff } = await octokit.pulls.get({
  owner: "octokit",
  repo: "rest.js",
  pull_number: 1278,
  mediaType: {
    format: "diff"
  }
});
```

The [AbortController](https://developer.mozilla.org/en-US/docs/Web/API/AbortController) interface can be used to abort one or more requests as and when desired. When the request is initiated, an [AbortSignal](https://developer.mozilla.org/en-US/docs/Web/API/AbortSignal) instance can be passed as an option inside the request's options object. For usage in Node, the [`abort-controller`](https://github.com/mysticatea/abort-controller) package can be used.

```
const controller = new AbortController();
const { data: prDiff } = await octokit.pulls.get({
  owner: "octokit",
  repo: "rest.js",
  pull_number: 1278,
  request: {
    signal: controller.signal
  }
});
```

Use `controller.abort()` to abort the request when desired.

## Custom requests

To send custom requests you can use the lower-level `octokit.request()` method

```
octokit.request("GET /");
```

The `baseUrl`, headers and other defaults are already set. For more information on the `octokit.request()` API see [`octokit/request.js`](https://github.com/octokit/request.js/)

All the endpoint methods such as `octokit.repos.get()` are aliases of `octokit.request()` with pre-bound default options. So you can use the `@octokit/request` API to get the default options or get generic request option to use with your preferred request library.

```
const defaultOptions = octokit.repos.get.endpoint.DEFAULTS;
const requestOptions = octokit.repos.get.endpoint({
  owner: "octokit",
  repo: "rest.js"
});
```

Note that authentication is not applied when retrieving request options from the `*.endpoint` APIs.

## Pagination

All endpoint methods starting with `.list*` do not return all responses at once but instead return the first 30 items by default, see also [GitHub’s REST API pagination documentation](https://developer.github.com/v3/#pagination).

To automatically receive all results across all pages, you can use the `octokit.paginate()` method:

```
octokit
  .paginate("GET /repos/:owner/:repo/issues", {
    owner: "octokit",
    repo: "rest.js"
  })
  .then(issues => {
    // issues is an array of all issue objects
  });
```

`octokit.paginate()` accepts the same options as [`octokit.request()`](https://octokit.github.io/rest.js/#custom-requests). You can optionally pass an additional function to map the results from each response. The map must return a new value, usually an array with mapped data.

```
octokit
  .paginate(
    "GET /repos/:owner/:repo/issues",
    { owner: "octokit", repo: "rest.js" },
    response => response.data.map(issue => issue.title)
  )
  .then(issueTitles => {
    // issueTitles is now an array with the titles only
  });
```

To stop paginating early, you can call the `done()` function passed as 2nd argument to the response map function. Note that you still have to return the value you want to map the response to, otherwise the last response will be mapped to undefined.

```
octokit.paginate("GET /organizations", (response, done) => {
  if (response.data.find(issues => issue.body.includes("something"))) {
    done();
  }
  return response.data;
});
```

To paginate responses for one of the registered endpoint methods such as `octokit.issues.listForRepo()` you can use the [`.endpoint.merge()`](https://github.com/octokit/endpoint.js#endpointmerge) method registered for all endpoint methods:

```
const options = octokit.issues.listForRepo.endpoint.merge({
  owner: "octokit",
  repo: "rest.js"
});
octokit.paginate(options).then(issues => {
  // issues is an array of all issue objects
});
```

If your runtime environment supports async iterators (such as most modern browsers and Node 10+), you can iterate through each response

```
for await (const response of octokit.paginate.iterator(options)) {
  // do whatever you want with each response, break out of the loop, etc.
}
```

`octokit.paginate.iterator()` accepts the same options as `octokit.paginate()`.

## Hooks

You can customize Octokit’s request lifecycle with hooks. Available methods are

```
octokit.hook.before("request", async options => {
  validate(options);
});
octokit.hook.after("request", async (response, options) => {
  console.log(`${options.method} ${options.url}: ${response.status}`);
});
octokit.hook.error("request", async (error, options) => {
  if (error.status === 304) {
    return findInCache(error.headers.etag);
  }

  throw error;
});
octokit.hook.wrap("request", async (request, options) => {
  // add logic before, after, catch errors or replace the request altogether
  return request(options);
});
```

See [before-after-hook](https://github.com/gr2m/before-after-hook#hookcollectionapi) for more details on the 4 methods.

## Custom endpoint methods

You can register custom endpoint methods such as `octokit.repos.get()` using the `octokit.registerEndpoints(routes)` method

```
octokit.registerEndpoints({
  foo: {
    bar: {
      method: "PATCH",
      url: "/repos/:owner/:repo/foo",
      headers: {
        accept: "application/vnd.github.foo-bar-preview+json"
      },
      params: {
        owner: {
          required: true,
          type: "string"
        },
        repo: {
          required: true,
          type: "string"
        },
        baz: {
          required: true,
          type: "string",
          enum: ["qux", "quux", "quuz"]
        }
      }
    }
  }
});

octokit.foo.bar({
  owner: "octokit",
  repo: "rest.js",
  baz: "quz"
});
```

This is useful when you participate in private beta features and prefer the convenience of methods for the new endpoints instead of using [`octokit.request()`](https://octokit.github.io/rest.js/'#custom-requests').

## Plugins

You can customize and extend Octokit’s functionality using plugins

```
// index.js
const MyOctokit = require("@octokit/rest").plugin([
  require("./lib/my-plugin"),
  require("octokit-plugin-example")
]);

// lib/my-plugin.js
module.exports = (octokit, options = { greeting: "Hello" }) => {
  // add a custom method
  octokit.helloWorld = () => console.log(`${options.greeting}, world!`);

  // hook into the request lifecycle
  octokit.hook.wrap("request", async (request, options) => {
    const time = Date.now();
    const response = await request(options);
    octokit.log.info(
      `${options.method} ${options.url} – ${response.status} in ${Date.now() -
        time}ms`
    );
    return response;
  });
};
```

`.plugin` accepts a function or an array of functions.

We recommend using [Octokit’s log methods](https://octokit.github.io/rest.js/#logging) to help users of your plugin with debugging.

You can add new methods to the `octokit` instance passed as the first argument to the plugin function. The 2nd argument is the options object passed to the constructor when instantiating the `octokit` client.

```
const octokit = new MyOctokit({ greeting: "Hola" });
octokit.helloWorld();
// Hola, world!
```

## Throttling

When you send too many requests in too little time you will likely hit errors due to rate and/or abuse limits.

In order to automatically throttle requests as recommended in [GitHub’s best practices for integrators](https://developer.github.com/v3/guides/best-practices-for-integrators/), we recommend you install the [`@octokit/plugin-throttling` plugin](https://github.com/octokit/plugin-throttling.js).

The `throttle.onAbuseLimit` and `throttle.onRateLimit` options are required. Return `true` to automatically retry the request after `retryAfter` seconds.

```
const Octokit = require("@octokit/rest").plugin(
  require("@octokit/plugin-throttling")
);

const octokit = new Octokit({
  auth: "token " + process.env.TOKEN,
  throttle: {
    onRateLimit: (retryAfter, options) => {
      octokit.log.warn(
        `Request quota exhausted for request ${options.method} ${options.url}`
      );

      if (options.request.retryCount === 0) {
        // only retries once
        console.log(`Retrying after ${retryAfter} seconds!`);
        return true;
      }
    },
    onAbuseLimit: (retryAfter, options) => {
      // does not retry, only logs a warning
      octokit.log.warn(
        `Abuse detected for request ${options.method} ${options.url}`
      );
    }
  }
});
```

## Automatic retries

Many common request errors can be easily remediated by retrying the request. We recommend installing the [`@octokit/plugin-retry` plugin](https://github.com/octokit/plugin-retry.js) for Automatic retries in these cases

```
const Octokit = require("@octokit/rest").plugin(
  require("@octokit/plugin-retry")
);

const octokit = new Octokit();

// all requests sent with the `octokit` instance are now retried up to 3 times for recoverable errors.
```

## Logging

`Octokit` has 4 built-in log methods

1.  `octokit.log.debug(message[, additionalInfo])`
2.  `octokit.log.info(message[, additionalInfo])`
3.  `octokit.log.warn(message[, additionalInfo])`
4.  `octokit.log.error(message[, additionalInfo])`

They can be configured using the [`log` client option](https://octokit.github.io/rest.js/client-options). By default, `octokit.log.debug()` and `octokit.log.info()` are no-ops, while the other two call `console.warn()` and `console.error()` respectively.

This is useful if you build reusable [plugins](https://octokit.github.io/rest.js/#plugins).

## Debug

The simplest way to receive debug information is to set the [`log` client option](https://octokit.github.io/rest.js/client-options) to `console`.

```
const octokit = require("@octokit/rest")({
  log: console
});

octokit.request("/");
```

This will log

```
request { method: 'GET',
  baseUrl: 'https://api.github.com',
  headers:
   { accept: 'application/vnd.github.v3+json',
     'user-agent':
      'octokit.js/0.0.0-development Node.js/10.15.0 (macOS Mojave; x64)' },
  request: {},
  url: '/' }
GET / - 200 in 514ms
```

If you like to support a configurable log level, we recommend using the [console-log-level](https://github.com/watson/console-log-level) module

```
const octokit = require("@octokit/rest")({
  log: require("console-log-level")({ level: "info" })
});

octokit.request("/");
```

This will only log

```
GET / - 200 in 514ms
```

## activity

### List public events

We delay the public events feed by five minutes, which means the most recent event returned by the public events API actually occurred at least five minutes ago.

```
octokit.activity.listPublicEvents()
```