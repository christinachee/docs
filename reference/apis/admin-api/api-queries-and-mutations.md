---
description: >-
  In this section of the Authgear documentation, you'll learn about all the
  GraphQL queries and mutations the Admin API supports.
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: false
  outline:
    visible: true
  pagination:
    visible: true
---

# Admin API Examples

Authgear provides a GraphQL API that you can use to manage users and other resources right from your application or using the GraphiQL Explorer in Authgear Portal.

The following section shows a detailed description and examples of supported queries and mutations.

## 1. Queries

### 1.1. auditLogs

The auditLogs query returns a list of all activities (logs) from the audit log.

**Schema:**

```graphql
auditLogs(
first: Int
last: Int
userIDs: [ID!]
sortDirection: SortDirection
before: String
after: String
rangeFrom: DateTime
rangeTo: DateTime
activityTypes: [AuditLogActivityType!]
): AuditLogConnection
```

**Example:**

{% tabs %}
{% tab title="Query" %}
```graphql
query {
  auditLogs(first: 4) {
    edges {
      node {
        id
        activityType
        createdAt
      }
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "auditLogs": {
      "edges": [
        {
          "node": {
            "activityType": "USER_AUTHENTICATED",
            "createdAt": "2023-09-11T09:23:51Z",
            "id": "QXVkaXRMb2c6MDAwMDAwMDAwMDA0ZDVjOQ"
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

### 1.2. users

You can use this query to fetch all registered users on your application. The users query returns a list of type User.

**Schema:**

```graphql
users(
first: Int
last: Int
searchKeyword: String
sortBy: UserSortBy
sortDirection: SortDirection
before: String
after: String
): UserConnection
```

**Example:**

{% tabs %}
{% tab title="Query" %}
```graphql
query {
  users(first: 2) {
    edges {
      node {
        id
        standardAttributes
      }
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "users": {
      "edges": [
        {
          "node": {
            "id": "VXNlcjo4ZGM4ZDgyjjkoKA0LTRjZGEtODZiOC03OTY4MGUwYzA5OGM",
            "standardAttributes": {
              "email": "myuser@gmail.com",
              "email_verified": true,
              "family_name": "John",
              "given_name": "Doe",
              "updated_at": 1686820949
            }
          }
        },
        {
          "node": {
            "id": "VXNlcjplMzA3OTAyaxKJuILTRjMjQtOTFjMS1jMmNkNjNhNmE0YWY",
            "standardAttributes": {
              "email": "user2@gmail.com",
              "email_verified": true,
              "family_name": "Eliano",
              "given_name": "Don",
              "updated_at": 1694359032
            }
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

### 1.3. node

A node represents a single object of different Types. The node query allows you to query a single object using the node ID. You learn more about node ID here: [https://docs.authgear.com/reference/apis/admin-api/node-id](https://docs.authgear.com/reference/apis/admin-api/node-id).

**Schema:**

```graphql
node(id: ID!): Node
```

**Example:**

You can specify different object types to the node query to fetch an item of that type. Examples of node Types include `User`, `AuditLog`, `Session`, `Authenticator`, `Authorization`, and `Identity`.

The following example uses the AuditLog node type.

{% tabs %}
{% tab title="Query" %}
```graphql
query {
  node(id: "QXVkaXRMb2c6MDAwHJKwMDAwMDA0ZDViOQ") {
    id
    ... on AuditLog {
      id
      activityType
      createdAt
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "node": {
      "activityType": "USER_AUTHENTICATED",
      "createdAt": "2023-09-11T09:23:51Z",
      "id": "QXVkaXRMb2c6MDAwHJKwMDAwMDA0ZDViOQ"
    }
  }
}
```
{% endtab %}
{% endtabs %}

### 1.4. nodes

The nodes query returns a list of nodes. This works similarly to the node query except that instead of supplying a single ID, you can provide a list of IDs for the objects you are querying for.

**Schema:**

```graphql
nodes(ids: [ID!]!): [Node]!
```

**Example:**

{% tabs %}
{% tab title="Query" %}
```graphql
query {
  nodes(ids: ["<NODE ID1>","<NODE ID2>"]) {
    id
    ... on AuditLog {
      id
      activityType
      createdAt
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "nodes": [
      {
        "activityType": "USER_AUTHENTICATED",
        "createdAt": "2023-09-11T09:23:51Z",
        "id": "QXKytXRMb4n6MDAwMDAwMDAwMDA0ZDVjUK"
      },
      {
        "activityType": "USER_PROFILE_UPDATED",
        "createdAt": "2023-09-14T06:57:27Z",
        "id": "QXVkaXABb7c6MDAwMDAwMDAwMDA0ZGKkZA"
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

## 2. Mutations

With mutations, you can modify data from your application using the Admin API GraphQL. For example, you can use mutation to update

### 2.1. anonymizeUser

Calling this mutation will change a specific user account to an anonymous account. In other words, this query anonymizes a specific user. This action will delete the user's data like name and gender.

**Schema:**

```graphql
anonymizeUser(input: AnonymizeUserInput!): AnonymizeUserPayload!
```

**Example:**

{% tabs %}
{% tab title="Query" %}
```
mutation {
  anonymizeUser(input: {userID: "<ENCODED USER ID>"}) {
    anonymizedUserID
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "anonymizeUser": {
      "anonymizedUserID": "XYQlcjo4ZGM4ZDg5OC1jNjA0ERRjZGEtODZiOC134TY4MGUwYzA5OGM"
    }
  }
}
```
{% endtab %}
{% endtabs %}

### 2.2. createIdentity

The createIdentity mutation creates a new identity for a user.

**Schema:**

```graphql
createIdentity(input: CreateIdentityInput!): CreateIdentityPayload!
```

**Example:**

{% tabs %}
{% tab title="Query" %}
```graphql
mutation {
  createIdentity(input: {userID: "<ENCODED USER ID>", definition: {loginID: {key: "email", value: "user@gmail.com"}}, password: "@x1ujD-9$"}) {
    identity {
      id
      claims
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "createIdentity": {
      "identity": {
        "claims": {
          "email": "user@gmail.com",
          "https://authgear.com/claims/login_id/key": "email",
          "https://authgear.com/claims/login_id/original_value": "user@gmail.com",
          "https://authgear.com/claims/login_id/type": "email",
          "https://authgear.com/claims/login_id/value": "user@gmail.com"
        },
        "id": "SWRlbnRpdHk6YjHiZGVhNjctABCwMy00OWU2LWIyOTMtNTIwMGU3KKUkMTBl"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### 2.3. createUser

The createUser mutation makes it possible to create a new user account from the Admin API.

**Schema:**

```graphql
createUser(input: CreateUserInput!): CreateUserPayload!
```

**Example:**

{% tabs %}
{% tab title="Query" %}
```graphql
mutation {
  createUser(input: {definition: {loginID: {key: "email", value: "user@gmail.com"}}, password:"my$ecurepa55"}) {
    user{
      id
      standardAttributes
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "createUser": {
      "user": {
        "id": "VXNlciklODRkMzdjZi1hZDQ5LTRiZDItOTMzZJ2tOGY1YThlYjc34RE",
        "standardAttributes": {
          "email": "user@gmail.com",
          "email_verified": false,
          "updated_at": 1694713743
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### 2.4. deleteAuthenticator

This mutation deletes an authenticator for a specific user.

**Schema:**

```graphql
deleteAuthenticator(input: DeleteAuthenticatorInput!): DeleteAuthenticatorPayload!
```

**Example:**

{% tabs %}
{% tab title="Query" %}
```graphql
mutation {
  deleteAuthenticator(input: {authenticatorID: "<ENCODED AUTHENTICATOR ID>"}) {
    user {
      authenticators {
        edges {
          node {
            id
          }
        }
      }
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
    "deleteAuthenticator": {
        "user": {
            "authenticators": {
                "edges": [
                    {
                        "node": {
                            "id": "QXV0aGVudGljYXRvcjpkGHczOGM0Yy0yNmY2LTQyOWMtODc0OS1kYTA3NjYxZjE0ABC"
                        }
                    }
                ]
            }
        }
    }
}
```
{% endtab %}
{% endtabs %}

### 2.5. deleteAuthorization

You can use the deleteAuthorization mutation to delete an existing authorization for a user.

**Schema:**

```graphql
deleteAuthorization(input: DeleteAuthorizationInput!): DeleteAuthorizationPayload!
```

**Example:**

{% tabs %}
{% tab title="Query" %}
```graphql
mutation {
  deleteAuthorization(input: {authorizationID: "<ENCODED AUTHORIZATION ID>"}) {
    user {
      authorizations {
        edges {
          node {
            id
          }
        }
      }
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
    "deleteAuthorization": {
        "user": {
            "authorizations": {
                "edges": [
                    {
                        "node": {
                            "id": "QXV0aG9yaXphdGlvbjpkHFczOGM0Yy0yNmY2LTQyOWMtODc0OS1kYTA3NjYxZjE0EFG"
                        }
                    }
                ]
            }
        }
    }
}
```
{% endtab %}
{% endtabs %}

### 2.6. deleteIdentity

The deleteIdentity mutation deletes the identity of a user.

**Schema:**

```graphql
deleteIdentity(input: DeleteIdentityInput!): DeleteIdentityPayload!
```

**Example:**

{% tabs %}
{% tab title="Query" %}
```graphql
mutation {
  deleteIdentity(
    input: {identityID: "<ENCODED IDENTITY ID>"}) {
    user {
      identities {
        edges {
          node {
            id
          }
        }
      }
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "deleteIdentity": {
      "user": {
        "identities": {
          "edges": [
            {
              "node": {
                "id": "SWRlbgsgdggGj7776JJkDc1My00ZTM2LWEyNTktZjg0ZjUyOER4NWJi"
              }
            }
          ]
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### 2.7. deleteUser

This mutation allows you to delete a specific user using the Admin API.

**Schema:**

```graphql
deleteUser(input: DeleteUserInput!): DeleteUserPayload!
```

**Example:**

{% tabs %}
{% tab title="Query" %}
```graphql
mutation {
  deleteUser(input: { userID: "<ENCODED USER ID>"}) {
    deletedUserID
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "deleteUser": {
      "deletedUserID": "VXNxcjowOKKcMzdjZi1hZDQ5LTRiZDItOTMzZC0yOGY1YThlYja86DQ"
    }
  }
}
```
{% endtab %}
{% endtabs %}

### 2.8. generateOOBOTPCode

Calling the generateOOBOTPCode mutation will generate a new OOB OTP Code for a user. This mutation allows you to specify the purpose and target of the OTP as input.

**Schema:**

```graphql
generateOOBOTPCode(input: GenerateOOBOTPCodeInput!): GenerateOOBOTPCodePayload!
```

**Example:**

{% tabs %}
{% tab title="Query" %}
```graphql
mutation {
  generateOOBOTPCode(input: {purpose: LOGIN, target: "user@gmail.com"}) {
    code
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "generateOOBOTPCode": {
      "code": "552660"
    }
  }
}
```
{% endtab %}
{% endtabs %}

### 2.9. resetPassword

The resetPassword mutation lets you rest a user's password from the Admin API.

**Schema:**

```graphql
resetPassword(input: ResetPasswordInput!): ResetPasswordPayload!
```

**Example:**

{% tabs %}
{% tab title="Query" %}
```graphql
mutation {
  resetPassword(input: {userID: "<ENCODED USER ID>", password: "n3w-p4$s"}) {
    user {
      id
      standardAttributes
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "resetPassword": {
      "user": {
        "id": "VXNlcjowNGUyJJO4Mi04NmEzLTRjYjItOGQxNy14ZWU0Y2FlNzQ5Kse",
        "standardAttributes": {
          "email": "user@gmail.com",
          "email_verified": false,
          "updated_at": 1694742340
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### 2.10. revokeAllSessions

With the revokeAllSessions mutation, you can revoke all sessions for a specific user.

**Schema:**

```graphql
revokeAllSessions(input: RevokeAllSessionsInput!): RevokeAllSessionsPayload!
```

**Example:**

{% tabs %}
{% tab title="Query" %}
```graphql
mutation {
  revokeAllSessions(input: {userID: "<ENCODED USER ID>"}) {
    user {
      id
      standardAttributes
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "revokeAllSessions": {
      "user": {
        "id": "VXNlcjowNGUyJJO4Mi04NmEzLTRjYjItOGQxNy14ZWU0Y2FlNzQ5Kse",
        "standardAttributes": {
          "email": "user@gmail.com",
          "email_verified": false,
          "updated_at": 1694742340
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### 2.11. revokeSession

This mutation revokes a specific user session. You can specify the session using the session ID.

**Schema:**

```graphql
revokeSession(input: RevokeSessionInput!): RevokeSessionPayload!
```

**Example:**

{% tabs %}
{% tab title="Query" %}
```graphql
mutation {
  revokeSession(input: {sessionID: "<ENCODED SESSION ID>"}) {
    user {
      id
      standardAttributes
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "revokeSession": {
      "user": {
        "id": "VXNlcjowNGUyJJO4Mi04NmEzLTRjYjItOGQxNy14ZWU0Y2FlNzQ5Kse",
        "standardAttributes": {
          "email": "user@gmail.com",
          "email_verified": false,
          "updated_at": 1694742340
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### 2.12. scheduleAccountAnonymization

The scheduleAccountAnonymization mutation provides a means to schedule a user account anonymization from the Admin API.

**Schema:**

```graphql
scheduleAccountAnonymization(input: ScheduleAccountAnonymizationInput!): ScheduleAccountAnonymizationPayload!
```

**Example:**

{% tabs %}
{% tab title="Query" %}
```graphql
mutation {
  scheduleAccountAnonymization(input: {userID: "<ENCODED USER ID>"}) {
    user {
      id
      standardAttributes
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```
{
  "data": {
    "scheduleAccountAnonymization": {
      "user": {
        "id": "VXNlcjowNGUyJJO4Mi04NmEzLTRjYjItOGQxNy14ZWU0Y2FlNzQ5Kse",
        "standardAttributes": {
          "email": "user@gmail.com",
          "email_verified": false,
          "updated_at": 1694742340
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### 2.13. scheduleAccountDeletion

The scheduleAccountDeletion mutation provides a means to schedule a user account deletion from the Admin API.

**Schema:**

<pre class="language-graphql"><code class="lang-graphql"><strong>scheduleAccountDeletion(input: ScheduleAccountDeletionInput!): ScheduleAccountDeletionPayload!
</strong></code></pre>

**Example:**

{% tabs %}
{% tab title="Query" %}
```graphql
mutation {
  scheduleAccountDeletion(input: {userID: "<ENCODED USER ID>"}) {
    user {
      id
      standardAttributes
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "scheduleAccountDeletion": {
      "user": {
        "id": "VXNlcjowNGUyJJO4Mi04NmEzLTRjYjItOGQxNy14ZWU0Y2FlNzQ5Kse",
        "standardAttributes": {
          "email": "user@gmail.com",
          "email_verified": false,
          "updated_at": 1694742340
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### 2.14. sendResetPasswordMessage

You can send a password reset message to a user from the Admin API using the sendResetPasswordMessage mutation.

**Schema:**

```graphql
sendResetPasswordMessage(input: SendResetPasswordMessageInput!): Boolean
```

**Example:**

{% tabs %}
{% tab title="Query" %}
```graphql
mutation {
  sendResetPasswordMessage(input: {loginID: "<USER LOGIN ID LIKE EMAIL>"})  
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "sendResetPasswordMessage": null
  }
}
```
{% endtab %}
{% endtabs %}

### 2.15. setDisabledStatus

The setDisabledStatus mutation enables you to enable or disable a user's account.

**Schema:**

```graphql
setDisabledStatus(input: SetDisabledStatusInput!): SetDisabledStatusPayload!
```

**Example:**

{% tabs %}
{% tab title="Query" %}
```graphql
mutation {
  setDisabledStatus(input: {userID: "<ENCODED USER ID>", isDisabled: true, reason: "Test"}) {
    user {
      id
      isDeactivated
    }
  } 
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "setDisabledStatus": {
      "user": {
        "id": "VXNlcjowNGUyJJO4Mi04NmEzLTRjYjItOGQxNy14ZWU0Y2FlNzQ5Kse",
        "isDeactivated": false
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### 2.16. setVerifiedStatus

You can use the setVerifiedStatus mutation to set a user as verified and unveried from the Admin API.

**Schema:**

```graphql
setVerifiedStatus(input: SetVerifiedStatusInput!): SetVerifiedStatusPayload!
```

**Example:**

{% tabs %}
{% tab title="Query" %}
```graphql
mutation {
  setVerifiedStatus(input: {userID: "<ENCODED USER ID>", claimName: "email", claimValue: "user@gmail.com", isVerified: true}) {
    user {
      id
      verifiedClaims {
        name
        value
      }
    }
  } 
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "setVerifiedStatus": {
      "user": {
        "id": "VXNlcjowNGUyJJO4Mi04NmEzLTRjYjItOGQxNy14ZWU0Y2FlNzQ5Kse",
        "verifiedClaims": [
          {
            "name": "email",
            "value": "myapkneeds@gmail.com"
          }
        ]
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### 2.17. unscheduleAccountAnonymization

This mutation allows you to cancel a previously scheduled mutation.

**Schema:**

```graphql
unscheduleAccountAnonymization(input: UnscheduleAccountAnonymizationInput!): UnscheduleAccountAnonymizationPayload!
```

**Example:**

{% tabs %}
{% tab title="Query" %}
```graphql
mutation {
  unscheduleAccountAnonymization(input: {userID: "<ENCODED USER ID>"}) {
    user {
      id
    }
  } 
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "unscheduleAccountAnonymization": {
      "user": {
        "id": "VXNlcjowNGUyJJO4Mi04NmEzLTRjYjItOGQxNy14ZWU0Y2FlNzQ5Kse"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### 2.18. unscheduleAccountDeletion

This mutation allows you to cancel a previously scheduled deletion.

**Schema:**

```graphql
unscheduleAccountDeletion(input: UnscheduleAccountDeletionInput!): UnscheduleAccountDeletionPayload!
```

**Example:**

{% tabs %}
{% tab title="Query" %}
```
mutation {
  unscheduleAccountDeletion(input: {userID: "<ENCODED USER ID>"}) {
    user {
      id
    }
  } 
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "unscheduleAccountDeletion": {
      "user": {
        "id": "VXNlcjowNGUyJJO4Mi04NmEzLTRjYjItOGQxNy14ZWU0Y2FlNzQ5Kse"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### 2.19. updateIdentity

The updateIdentity mutation updates an existing identiy of a user.

**Schema:**

```graphql
updateIdentity(input: UpdateIdentityInput!): UpdateIdentityPayload!
```

**Example:**

{% tabs %}
{% tab title="Query" %}
```graphql
mutation {
  updateIdentity(input: { definition: {loginID: {key: "email", value: "user@gmail.com"}}, userID: "<ENCODED USER ID>", identityID: "<ENCODED IDENTITY ID>"}) {
    user {
      id
    }
  } 
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "updateIdentity": {
      "user": {
        "id": "VXNlcjowNGUyJJO4Mi04NmEzLTRjYjItOGQxNy14ZWU0Y2FlNzQ5Kse"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### 2.20. updateUser

You can use this mutation to update an existing user's details. You can update standard attributes such as email and phone for the user. Or you can modify custom fields using the `customAttributes` argument.

**Schema:**

```graphql
updateUser(input: UpdateUserInput!): UpdateUserPayload!
```

**Example:**

For this updateUser example, we will be updating the standard attributes for a user. The first thing to do is to extract all the current values of the user's standard attributes into a variable. Then, add new fields or modify existing fields in the variable with new values.

The following block of code shows an example variable. If you're using GraphiQL, simply create the variable in the variable tab of GraphiQL like this:

```json
{
  "standardAttributes": {
    "family_name": "John",
    "given_name": "Doe",
    "gender": "male"
  }
}
```

{% tabs %}
{% tab title="Query" %}
```graphql
mutation ($standardAttributes: UserStandardAttributes) {
  updateUser(input: {userID: "<ENCODED USER ID>", standardAttributes: $standardAttributes}) {
    user {
      id
      standardAttributes
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "updateUser": {
      "user": {
        "id": "VXNlcjowNGUyJJO4Mi04NmEzLTRjYjItOGQxNy14ZWU0Y2FlNzQ5Kse",
        "standardAttributes": {
          "email": "user@gmail.com",
          "email_verified": true,
          "family_name": "John",
          "gender": "male",
          "given_name": "Doe",
          "updated_at": 1694947082
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}





