---
description: >-
  Authgear exposes APIs for developers to manage their applications
  programmatically
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# APIs

Besides the Client SDKs, Authgear expose the following APIs for simple integration with your applications for authentication and user managements.

All of these are on the endpoint of your app. The default endpoint is at `https://[myapp].authgear.cloud` unless you setup a custom domains. `[myapp]` is your project name.

Unless otherwise specified, all paths mentioned here are relative to the endpoint of your app.

Authgear provides the following groups of APIs:

* **OAuth 2.0 and OpenID Connect**: for connecting with OIDC Clients
* **Admin API**: for your servers to manage users via a GraphQL endpoint.
* **AuthFlow API**: for developing a customized Web or Mobile Native Auth UI instead of the default provided by Authgear
* **Resolver Endpoint**: for API Gateway or Servers to check the valiadity of access tokens or cookies in the request header.

Here are all of the special path with each group of the API above.

## OAuth 2.0 and OpenID Connect

For more information about OIDC API endpoint, please refer to the following sections, or anyone of the Regular Web App getting started guide.

{% content-ref url="oauth-2.0-and-openid-connect-oidc/" %}
[oauth-2.0-and-openid-connect-oidc](oauth-2.0-and-openid-connect-oidc/)
{% endcontent-ref %}

{% content-ref url="../../how-to-guide/authenticate/oidc-provider.md" %}
[oidc-provider.md](../../how-to-guide/authenticate/oidc-provider.md)
{% endcontent-ref %}

The related URLs are:

* `/.well-known/openid-configuration`\
  \
  This endpoint serves as a JSON document containing the OpenID Connect configuration of your app. That includes the authorization endpoint, the token endpoint, and the JWKs endpoint. Here is [an example of how it looks](https://accounts.portal.authgear.com/.well-known/openid-configuration).
* `/.well-known/oauth-authorization-server`\
  \
  This endpoint serves a JSON document containing the authorization server metadata of your app. That includes the authorization endpoint, the token endpoint, and the JWKs endpoint. Here is [an example of how it looks](https://accounts.portal.authgear.com/.well-known/openid-configuration).
* `/oauth2/userinfo`\
  \
  The UserInfo Endpoint is an OAuth 2.0 Protected Resource that returns Claims about the authenticated End-User. When the client presents with a valid Access Token, the endpoint responds with the claims packaged in a JSON object. The claims are also the attributes of the [User Profile](../../how-to-guide/user-management/user-profile.md).

## Admin API

Please refer to the following documentations:

{% content-ref url="admin-api/" %}
[admin-api](admin-api/)
{% endcontent-ref %}

The Admin API are under:

* `/_api/admin/graphql`

## AuthFlow API

Please refer to the following documentations:

{% content-ref url="../../how-to-guide/custom-ui/authentication-flow-api.md" %}
[authentication-flow-api.md](../../how-to-guide/custom-ui/authentication-flow-api.md)
{% endcontent-ref %}

The Authentication Flow API are under:

`api/v1/authentication_flows`

## Resolver Endpoints

The resolver end point at the following URLs:

* `/_resolver/resolve`

The endpoint serves as a resolver to check the access token or cookie in the request headers. Forward incoming HTTP requests to this endpoint and the resolver will add the `x-authgear-` headers the to response.

See the list of `x-authgear-` headers in the specs [here](https://github.com/authgear/authgear-server/blob/master/docs/specs/api-resolver.md).

See implementation examples [here](../../get-started/backend-integration/nginx.md).

A tutorial is available below, shall you choose to use Resolver Endpoints instead of JWT tokens to validate each API requests:

{% content-ref url="../../get-started/backend-integration/nginx.md" %}
[nginx.md](../../get-started/backend-integration/nginx.md)
{% endcontent-ref %}

## Other special URLs

Here are two other URLs

* `/`\
  \
  This endpoint is the entry point of the Web UI. You can visit it if you want to try your configuration (only for custom domains). However, this is NOT the authorization endpoint. You must use our SDK to initiate the authentication.
* `/settings`\
  \
  User settings UI
