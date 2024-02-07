---
description: >-
  Learn about what the Authentication Flow API is and how to use it to implement
  your own custom user interface (UI) for authentication.
---

# Authentication Flow API

Build your own custom UI for login, signup, and more powered by the Authentication Flow API.

By default, when you set up an Authgear project, it makes use of the regular AuthUI offered by Authgear. Although AuthUI offers many customization options such as the ability to change the theme and color or add your brand logo to authentication pages, you might have more needs. With the Authentication Flow API, you can build your own authentication UI from the ground up, and using your preferred programming language and tool.

### What is the Authentication Flow API?

The Authentication Flow API is an REST HTTP API that you can use to create and run an authentication flow. You can use this API to create your own custom UI for pages like login, signup, account recovery, 2FA, and more.

To use the API on its own you make an HTTP request to one of the valid endpoints as shown below:

**Endpoint:** `{your-authgear-project-domain}/api/v1/authentication_flows`

**Request method:** POST

**Request header:**

```json
{
    "Content-Type": "application/json",
    "Accept": "application/json",
}
```

All requests to the API should include the above header.

**Request body:**

```json
{
    "type": "login",
    "name": "default"
}
```

The following is a sample of the response you would get from the Authentication Flow API for the above request:

```json
{
    "result": {
        "state_token": "authflowstate_VGHZ8SBCKGZK2KW84TCAKWGM8QZH0B69",
        "type": "login",
        "name": "default",
        "action": {
            "type": "identify",
            "data": {
                "options": [
                    {
                        "identification": "phone"
                    },
                    {
                        "identification": "email"
                    },
                    {
                        "identification": "oauth",
                        "provider_type": "google",
                        "alias": "google"
                    }
                ]
            }
        }
    }
}
```

The above response means you've successfully started a new login flow using the API.

To continue and finish the authentication flow, you can send the value for `state_token` from the above response in your next HTTP request to the `api/v1/authentication_flows/states/input` endpoint like the example shown below:

**Endpoint:** `{your-authgear-project-domain}/api/v1/authentication_flows/states/input`

**Request method:** POST

**Request body:**

```json
{
    "state_token": "authflowstate_VGHZ8SBCKGZK2KW84TCAKWGM8QZH0B69",
    "batch_input": [
      {
            "identification": "email",
            "login_id": "user@example.com"
      },
      {
          "authentication": "primary_password",
          "password": "12345678"
      }
    ]
}
```

In this second request (or second step of the authentication flow), we use the `state_token` to pass the user ID and password for the user we want to log in to our application.

You may send more requests just like this second request depending on the number of steps required for your specific authentication flow.

To use the Authentication Flow API to build your custom UI, you need to configure a Custom UI URI in the Authgear portal. This URI should point to your custom authentication page.

### How the Authentication Flow API Works

The following flowchart shows the steps in a simple Authentication Flow API implementation:

<figure><img src="../../.gitbook/assets/authflow-api-flowchart.png" alt=""><figcaption><p>flowchart showing how authentication flow API works</p></figcaption></figure>

#### Step 1: Make Authorization Request

When you use the authentication flow API to power your custom UI, the authentication flow for your app will start with your app sending an authorization request to Authgear's authorization server as shown in **step 1** in the figure above.

#### Step 2: Redirect to Custom UI

If you have Authentication Flow API enabled and a Custom UI URI set for your Authgear App, the authorization server will redirect your users to the custom UI as shown in **step 2** above.

#### Step 3: Make HTTP Requests to Authentication Flow API Endpoints

In your custom UI, implement the code that interacts with the Authentication Flow API (using HTTP requests) as demonstrated in **step 3** of the above figure.

#### Step 4: Finish the Authentication Flow

In an authentication flow with multiple steps, the custom UI and Authgear's authentication server may perform **steps 3** and **4** in the above figure multiple times. In such cases, only the final response at the end of the authentication flow will include a **finish\_redirect\_url**.

You must redirect your user to the **finish\_redirect\_url** to complete the authentication flow.

To dive deeper into how to use the Authentication Flow API to power your custom UI, check out the following tutorials for your preferred programming languages and frameworks.

{% content-ref url="implement-authentication-flow-api-using-php.md" %}
[implement-authentication-flow-api-using-php.md](implement-authentication-flow-api-using-php.md)
{% endcontent-ref %}

{% content-ref url="implement-authentication-flow-api-using-express.md" %}
[implement-authentication-flow-api-using-express.md](implement-authentication-flow-api-using-express.md)
{% endcontent-ref %}
