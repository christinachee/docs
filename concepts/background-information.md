---
description: Implement Authgear to control access to your applications
---

# Background Information

Authgear consists of these major parts:

* **Authgear server**
  * It is an OpenID Connect (OIDC) compatible authentication service
  * It provides a wide range of authentication and user management features
  * The [Authgear Resolver Endpoint](../get-started/backend-integration/nginx.md) or [JWT access tokens](../get-started/backend-integration/jwt.md) can be used to authenticate incoming requests
* **Authgear SDKs**
  * Used for developers to integrate Authgear into their apps. It can be used to perform user authentication, get the user profile, send authenticated requests
  * Explore: [Web (JavaScript)](../get-started/single-page-app/website.md), [React Native](../get-started/react-native.md), [iOS](../get-started/ios.md), [Android](../get-started/android/), [Flutter](../get-started/flutter.md), [Xamarin](../get-started/xamarin.md)
* **Admin API**
  * For backend servers to perform administrative tasks. Most things about user management you can do in the Authgear Portal, you can do it with [Admin API](../reference/apis/admin-api/)
* **Authgear Portal**
  * You can use the Authgear Portal for configuring your project, managing users, checking [audit log](../how-to-guide/monitor/audit-log.md), and customizing the behavior by [event hooks](../integrate/events-hooks/).
* **AuthUI**
  * The prebuilt UI for end-users to complete authentication and perform [account settings](../integrate/auth-ui.md)
  * It can be [customized](../how-to-guide/customize/branding.md) to fit your company's branding
* **Events and Hooks**
  * Use [event and hooks](../integrate/events-hooks/) to get the information about events that happened (non-blocking) and change the process of the Authgear server (blocking)
