---
description: >-
  Learn about what the Authentication Flow API is and how to use it to implement
  your own custom user interface (UI) for authentication.
---

# Authentication Flow API

The Authentication Flow API is an HTTP API that you can use to create and run an authentication flow. You can use this API to create your own custom UI for pages like login, signup, account recovery, 2FA, and more.

To use the Authentication Flow API, you need to configure a Custom UI URI in the Authgear portal, and then implement code that makes HTTP requests to the API endpoints.

### Key Concepts in Authentication Flow API

The following are concepts worth noting before you start working with the API:

* **URL Query**: When any of the Authgear SDKs redirects users to your Custom UI URL, Authgear adds some additional queries to the URL. These queries are required in order to get the `finish_redirect_uri`.
* **Endpoint**: You can find your Authgear project domain from your application configuration page (usually under the **Endpoint** section) in Authgear Portal. The full endpoint is this Authgear domain followed by a valid path for an operation. For example, `https://my-project.authgear.cloud/api/v1/authentication_flows` to start an authentication flow. The two valid paths the API supports are `/api/v1/authentication_flows` and `/api/v1/authentication_flows/states/input` (which is for passing input to an existing authentication flow). The endpoints only support HTTP POST requests.
* **State Token**: The Authentication Flow API supports authentication with multi-step UI just like the default authentication flow in Auth UI. State tokens can be used to make this type of type of authentication flow work. You can pass information about a previous step to the next step by using the state token. For example, using the state token in the result of step A as input in step B to continue using the state of the previous step.
* **Inputs**: You can pass values to Authentication Flow API using the `input` or `batch_input` parameters in your HTTP request body. Use the `batch_input` to send multiple values as in an array and `input` when you are passing only 1 value. Inputs must be sent as JSON in your HTTP request body.
* **Finish Redirect URI**: A URL that you can use to redirect back to your app at the end of the authentication flow.

### How the Authentication Flow API Works

The following flowchart shows the steps in a simple Authentication Flow API implementation:

<figure><img src="../../.gitbook/assets/authflow-api-flowchart.png" alt=""><figcaption><p>flowchart showing how authentication flow API works</p></figcaption></figure>

When you use the authentication flow API to power your custom UI, the authentication flow for your app will start with your app sending an authorization request to Authgear's authorization server as shown in **step 1** in the figure above.

If you have Authentication Flow API enabled and a Custom UI URI set for your Authgear App, the authorization server will redirect your users to the custom UI as shown in **step 2** above.

In your custom UI, implement the code that interacts with the Authentication Flow API (using HTTP requests) as demonstrated in **step 3** of the above figure.

In an authentication flow with multiple steps, the custom UI and Authgear's authentication server may perform steps 3 and 4 in the above figure multiple times. In such cases, only the final response at the end of the authentication flow will include the **finish\_redirect\_url**.

The following tutorials show how to implement Authentication Flow API using different programming languages and frameworks.

{% content-ref url="implement-authentication-flow-api-using-php.md" %}
[implement-authentication-flow-api-using-php.md](implement-authentication-flow-api-using-php.md)
{% endcontent-ref %}

{% content-ref url="implement-authentication-flow-api-using-express.md" %}
[implement-authentication-flow-api-using-express.md](implement-authentication-flow-api-using-express.md)
{% endcontent-ref %}
