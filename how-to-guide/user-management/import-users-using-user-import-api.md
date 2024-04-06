---
description: >-
  Use the user import API to bulk import users from external systems to your
  Authgear project
---

# Import Users using User Import API

Some ways to add users to your Authgear project include; using the **Add User** UI in Authgear Portal, using the **createUser()** mutation in Admin API, and last but not least, having the user accounts created using sign-up page on AuthUI.

A common downside of all the above-listed methods is that they do not support batch import of users. Meaning, that you have to add users one by one. This isn't ideal for importing multiple users from existing legacy systems to Authgear. For adding bulk users, there is the User Import API.

In this post, you'll learn what the User Import API is and see examples of how to import bulk users to an Authgear project.

## User Import API

The User Import API is an API that supports the bulk import of users from another system to an Authgear project. This API is not part of the Admin API GraphQL. However, the API endpoints require the [Admin API JWT token](https://docs.authgear.com/reference/apis/admin-api/authentication-and-security) to access it.

The following are other **important things to note about the User Import API**:

* The actual process of importing the users is asynchronous. This means execution is done in the background. The API provides an endpoint developers can use to query the status of the import.
* Once an import is initiated successfully, the API will return an ID for the task. This ID is required to query the status of the import.
* The body of HTTP requests to the API has a limit of 500KB.
* Using the User Import API does not trigger the `user.pre_create` and `user.created` hooks.
* The API supports the Bcrypt password format. To import passwords using this format specify the format `type` and `password_hash` in an object that will be the value of the user's `password` field. For example:&#x20;

```json
"password": {
        "type": "bcrypt",
        "password_hash": "$2a$10$N9qo8uLOickgx2ZMRZoMyeIjZAgcfl7p92ldGxad68LJZdL17lhWy"
      },
```

### Endpoints

The Import User API has two endpoints, one for initiating a user import task and the other for checking the status of the task.  The endpoints only support secure HTTPS request and require a valid Admin API JWT token using `Bearer` authorization header  (`Authorization: Bearer <Admin API JWT Token>`).

Here are more details about the endpoints and their expected inputs.

#### Initiate Import

```
POST - /_api/admin/users/import
HTTP/1.1
Host: <Your Authgear Project domain>
Authorization: Bearer <Admin API JWT Token>
Content-type: application/json
Body: {
    "identifier": "email",
    "records": [
        {
            "email": "user@example.com",
            "email_verified": true,
            "password": {
                "type": "bcrypt",
                "password_hash": "$2a$10$N9qo8uLOickgx2ZMRZoMyeIjZAgcfl7p92ldGxad68LJZdL17lhWy"
            }
        }
    ]
}

```

#### Check Status

```
GET - /_api/admin/users/import/{ID}
HTTP/1.1
Host: <Your Authgear Project domain>
Authorization: Bearer <Admin API JWT Token>
```

### Input Format

The [endpoint that initiates an import](import-users-using-user-import-api.md#initiate-import) accepts JSON input via an HTTP request body. The following sample JSON document shows the expected structure and fields of the input:

```json
{
  "upsert": true,
  "identifier": "email",
  "records": [
    {
      "preferred_username": "jdoe",
      "email": "johndoe@example.com",
      "phone_number": "+85123456789",

      "email_verified": true,
      "phone_number_verified": true,

      "name": "John Doe",
      "given_name": "John",
      "family_name": "Doe",
      "middle_name": "",
      "nickname": "JD",
      "profile": "https://example.com",
      "picture": "https://example.com",
      "website": "https://example.com",
      "gender": "male",
      "birthdate": "1990-01-01",
      "zoneinfo": "Asia/Hong_Kong",
      "locale": "zh-Hant-HK",
      "address": {
        "formatted": "1 Unnamed Road, Central, Hong Kong Island, HK",
        "street_address": "1 Unnamed Road",
        "locality": "Central",
        "region": "Hong Kong",
        "postal_code": "N/A",
        "country": "HK"
      },

      "custom_attributes": {
        "member_id": "123456789"
      },

      "roles": ["role_a", "role_b"],
      "groups": ["group_a"],

      "disabled": false,

      "password": {
        "type": "bcrypt",
        "password_hash": "$2a$10$N9qo8uLOickgx2ZMRZoMyeIjZAgcfl7p92ldGxad68LJZdL17lhWy"
      },

      "mfa": {
        "email": "johndoe@example.com",
        "phone_number": "+85123456789",
        "password": {
          "type": "bcrypt",
          "password_hash": "$2a$10$N9qo8uLOickgx2ZMRZoMyeIjZAgcfl7p92ldGxad68LJZdL17lhWy"
        },
        "totp": {
          "secret": "secret"
        }
      }
    }
  ]
}
```

To understand the input better, let's take a close look at the three fields (`upsert`, `identifier`, `records`) that are directly on the root of the above JSON document.

* `upsert`: This is an optional boolean that is `false` by default. When the value for this field is set to `true` and a user already exists with the same identity, the user's data is updated based on the [update behavior](import-users-using-user-import-api.md#update-behavior) for each attribute.
* `identifier`: This field is required. It tells Authgear what attribute to use to identify an existing user. The following strings are the accepted values: `preferred_username`, `email`, and `phone_number`.
* `records`: This is where a developer can provide the data of all the users they wish to import in an array. Each direct object in the array represents a single user. Within the object, you can define the standard attributes for the user using the various fields as shown in the sample above. You may also define custom attributes in an object nested inside the `custom_attributes` field as also shown above.

### Update Behavior

The update behavior for an attribute determines how Authgear will treat that attribute when an existing user has the same value for the specified `identifier` type. For example, if the `identifier` is "email", the update behavior for each attribute is how Authgear will treat the attribute if a user already exists with the same email address as the current user you're trying to import.

Each attribute can have one of the three different types of update behavior described below:

* **UPDATED\_IF\_PRESENT\_AND\_REMOVED\_IF\_NULL**: An attribute with this update behavior will update the user's attribute to the new value if that new value is **not null**. If the new value is explicitly **null**, the attribute will be deleted for the user. And if the attribute is absent, no operation is done.
* **UPDATED\_IF\_PRESENT**: When this is the update behavior of an attribute, it will be updated if it is present. If the attribute is not present, no operation is done.
* **IGNORED**: If a user exists already, the new value of this attribute is ignored. If the attribute is absent, nothing is done.



#### Update Behavior of each field

The following table shows all attributes and their update behavior for reference purposes:

| Item                                          | Update Behavior                                  | Description                                                                                                                                                                                                            |
| --------------------------------------------- | ------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `preferred_username`, `email`, `phone_number` | **UPDATED\_IF\_PRESENT\_AND\_REMOVED\_IF\_NULL** | If it is not `identifier`, then the update behavior applies. The corresponding Login ID will be created, updated or removed as needed.                                                                                 |
| `email_verified`, `phone_number_verified`     | **UPDATED\_IF\_PRESENT**                         | For example, in the first import, if `email_verified` is absent, then email is marked as unverified.                                                                                                                   |
| All other standard attributes                 | **UPDATED\_IF\_PRESENT\_AND\_REMOVED\_IF\_NULL** | In particular, `address` IS NOT merged with the existing value, but REPLACES the existing `address` value.                                                                                                             |
| `custom_attributes.*`                         | **UPDATED\_IF\_PRESENT\_AND\_REMOVED\_IF\_NULL** | For each attribute in `custom_attributes`, the update behavior applies individually. So an absent custom attribute in an upsert does not change the existing value.                                                    |
| `roles`, `groups`                             | **UPDATED\_IF\_PRESENT**                         | If present, the roles and groups of the user will match the value. For example, supposed the user originally has `["role_a", "role_b"]`. `roles` is `["role_a", "role_c"]`. `role_b` is removed and `role_c` is added. |
| `disabled`                                    | **UPDATED\_IF\_PRESENT**                         | Re-importing a record without specifying `disabled` WILL NOT accidentally alter the disabled state previously set by other means.                                                                                      |
| `password`                                    | **IGNORED**                                      | If it was not provided when the record was first imported, subsequent import CANNOT add it back.                                                                                                                       |
| `mfa.email`                                   | **UPDATED\_IF\_PRESENT\_AND\_REMOVED\_IF\_NULL** | If provided, the user can perform 2FA with email OTP.                                                                                                                                                                  |
| `mfa.phone_number`                            | **UPDATED\_IF\_PRESENT\_AND\_REMOVED\_IF\_NULL** | If provided, the user can perform 2FA with phone OTP.                                                                                                                                                                  |
| `mfa.password`                                | **IGNORED**                                      | If it was not provided when the record was first imported, subsequent import CANNOT add it back.                                                                                                                       |
| `mfa.totp`                                    | **IGNORED**                                      | If it was not provided when the record was first imported, subsequent import CANNOT add it back.                                                                                                                       |

### Usage Example

In this section, you can find code for a simple example of using the User Import API to add multiple users to an Authgear project.

#### Pre-requisites

To follow this example and be able to run the code on your local machine, you must have the following:

* An Authgear account. Sign up for free [here](https://accounts.portal.authgear.com/signup).
* [Node.js](https://nodejs.org/) installation on your local computer.
* Install Express.js by running the following command from your project directory: `npm install express`.

#### Step 1: Get Admin API JWT

As mentioned earlier in this post, the User Import API requires the Admin API JWT to access.&#x20;

First, install JsonWebToken (a Node package for generating JWT) by running the following command:

```sh
npm install jsonwebtoken
```

The following code shows how to get the token:

```javascript
function generateJWT() {
    const project_id = ""; //Your authgear app id
    const key_id = ""; //you authgear key ID
    const expiresAt = Math.floor(Date.now() / 1000) + (60 * 60); //the current value means token will expire in 1 hour.
    
    //Payload to include in JWT
    const claims = {
        aud: project_id,
        iat: Math.floor(Date.now() / 1000) - 30,
        exp: expiresAt
    }
    const privateKey = fs.readFileSync("key.pem"); //Read value from the downloaded key file
    const header = { "typ": "JWT", "kid": key_id, "alg": "RS256" }
    const jwt = node_jwt.sign(claims, privateKey, { header: header });

    return jwt;
}
```

See our post on [Admin API Authentication](https://docs.authgear.com/reference/apis/admin-api/authentication-and-security) for a more detailed guide on how to get your key ID, and private key and generate Admin API JWT.

#### Step 2: Import Users from a JSON Document

In the following steps, we'll use the node-fetch package to make HTTP requests to the User Import API. Hence, install node-fetch by running the following command:

```sh
npm install node-fetch
```

The following code sample demonstrates how to import 2 users from a JSON document that's stored in a simple constant (`const data`):

```javascript
const express = require("express");
const node_jwt = require('jsonwebtoken');
const fs = require('fs');
const fetch = require('node-fetch');
const app = express();
const port = 3002;

//TODO Place declaration of generateJWT() function here

app.get('/', (request, response) => {

    const jwt = generateJWT();
    const data = {
        "identifier": "email",
        "records": [
            {
                "email": "user1@example.com",
                "email_verified": true,
                "password": {
                    "type": "bcrypt",
                    "password_hash": "$2a$10$N9qo8uLOickgx2ZMRZoMyeIjZAgcfl7p92ldGxad68LJZdL17lhWy"
                }
            },
            {
                "email": "user2@example.com",
                "email_verified": false,
                "name": "John Doe",
                "given_name": "John",
                "family_name": "Doe",
                "password": {
                    "type": "bcrypt",
                    "password_hash": "$2a$10$N9qo8uLOickgx2ZMRZoMyeIjZAgcfl7p92ldGxad68LJZdL17lhWy"
                }
            }
        ]
    }

    const options = {
        method: 'POST',
        headers: { 'Content-type': 'application/json', 'Authorization': 'Bearer ' + jwt },
        body: JSON.stringify(data)
    };

    const appUrl = 'https://your-project.authgearapps.com'; // replace wuth your authgear project url

    fetch(`${appUrl}/_api/admin/users/import`, options)
        .then(result => result.json())
        .then(result => console.log(JSON.stringify(result)));

    response.send("Request sent using the follow JWT as Bearer: " + jwt + "See console for result");
});

app.listen(port, () => {
    console.log("server started! PORT: " + port);
});
```

If the user import was initiated successfully, you'll get a response that looks like this:

```json
{
  "id": "task_4WZ0V7EPT4GZ2ABVN03QXYZ122W835C1",
  "created_at": "2024-04-04T06:56:36.02508096Z",
  "status": "pending"
}
```

In the next step, we'll use the value of the `id` field from the above response to query the status of the import task.

#### Step 3: Get the Status of the Import Task

Add a new route to the Express app that accepts the task `id` as a parameter and uses that id to query the status of the task. Here's the code for the route:

```javascript
app.get("/status/:id", (request, response) => {
    const jwt = generateJWT()

    const options = {
        method: 'GET',
        headers: { 'Content-type': 'application/json', 'Authorization': 'Bearer ' + jwt }
    };

    const appUrl = 'https://your-project.authgearapps.com';

    fetch(`${appUrl}/_api/admin/users/import/${request.params.id}`, options)
        .then(result => result.json())
        .then(result => console.log(JSON.stringify(result)));

    response.send("Request sent using the follow JWT as Bearer: " + jwt + "See console for result");
});
```

The response to the request to query the status of the import task will look like this:&#x20;

```json
{
  "id": "task_4WZ0V7EPT4GZ2ABVN03QXYZ122W835C1",
  "created_at": "2024-04-04T06:56:36.02508096Z",
  "status": "completed",
  "summary": {
    "total": 2,
    "inserted": 2,
    "updated": 0,
    "skipped": 0,
    "failed": 0
  },
  "details": [
    {
      "index": 0,
      "record": {
        "email": "user1@example.com",
        "email_verified": true,
        "password": {
          "password_hash": "REDACTED",
          "type": "bcrypt"
        }
      },
      "outcome": "inserted",
      "user_id": "0f0f65ee-4c7d-45a0-a740-bcbbfd3fcf06"
    },
    {
      "index": 1,
      "record": {
        "email": "user2@example.com",
        "email_verified": false,
        "family_name": "Doe",
        "given_name": "John",
        "name": "John Doe",
        "password": {
          "password_hash": "REDACTED",
          "type": "bcrypt"
        }
      },
      "outcome": "inserted",
      "user_id": "9c71fc29-6db6-4a18-aa73-774139fed16d",
      "warnings": [
        {
          "message": "email_verified = false has no effect in insert."
        }
      ]
    }
  ]
}
```

From the response, you can see the `status` of the entire task (import was `completed`), including a summary ( `{ "total": 2, "inserted": 2, "updated": 0, "skipped": 0, "failed": 0 }` ).

The `details` field contains an array of details such as the `outcome` for each user in the original JSON document.
