# Table of contents

* [Authgear Overview](README.md)

## Get Started

* [Start Building](get-started/start-building/README.md)
* [Single-Page App](get-started/single-page-app/README.md)
  * [JavaScript (Web)](get-started/single-page-app/website.md)
  * [React](get-started/single-page-app/react.md)
  * [Angular](get-started/single-page-app/angular.md)
  * [Vue](get-started/single-page-app/vue.md)
* [Native/Mobile App](get-started/start-building/native-mobile-app.md)
  * [iOS SDK](get-started/ios.md)
  * [Android SDK](get-started/android/README.md)
    * [Android Kotlin coroutine support](get-started/android/coroutine-support.md)
    * [Android OKHttp Interceptor Extension (Optional)](get-started/android/okhttp-interceptor-extension.md)
  * [Flutter SDK](get-started/flutter.md)
  * [React Native SDK](get-started/react-native.md)
  * [Xamarin SDK](get-started/xamarin.md)
* [Regular Web App](get-started/regular-web-app/README.md)
  * [Java Spring Boot](get-started/regular-web-app/java-spring-boot.md)
* [Backend/API](get-started/start-building/backend-api.md)
  * [Backend Integration](get-started/backend-integration/README.md)
  * [Forward Authentication to Authgear Resolver Endpoint](get-started/backend-integration/nginx.md)
  * [Validate JWT in your application server](get-started/backend-integration/jwt.md)
* [Choose your authentication approach](get-started/authentication-approach/README.md)
  * [Token-based (Native mobile or Single-page app)](get-started/authentication-approach/token-based.md)
  * [Cookie-based (Website or Single-page app)](get-started/authentication-approach/cookie-based.md)

## How-To Guides <a href="#how-to-guide" id="how-to-guide"></a>

* [Integrate](how-to-guide/integration/README.md)
  * [Access User Profiles](how-to-guide/integration/access-user-profiles.md)
  * [Add custom fields to a JWT Access Token](how-to-guide/integration/add-custom-fields-to-a-jwt-access-token.md)
* [Authenticate](how-to-guide/authenticate/README.md)
  * [Setup local development environment for Cookie-based authentication](how-to-guide/authenticate/local-cookie-based-web-setup.md)

## Concepts <a href="#how-to-guide" id="how-to-guide"></a>

* [Background Information](how-to-guide-1/background-information.md)

## Strategies

* [User, Identity and Authenticator](strategies/user-identity-and-authenticator.md)
* [Passkeys](strategies/passkeys.md)
* [WhatsApp OTP Login](strategies/whatsapp-otp-login.md)
* [Email Login Link](strategies/email-login-link.md)
* [Social & Enterprise Identity Providers](strategies/how-to-setup-sso-integrations/README.md)
  * [Connect Apps to Apple](strategies/how-to-setup-sso-integrations/apple.md)
  * [Connect Apps to Google](strategies/how-to-setup-sso-integrations/google.md)
  * [Connect Apps to Facebook](strategies/how-to-setup-sso-integrations/facebook.md)
  * [Connect Apps to GitHub](strategies/how-to-setup-sso-integrations/github.md)
  * [Connect Apps to LinkedIn](strategies/how-to-setup-sso-integrations/linkedin.md)
  * [Connect Apps to Azure Active Directory](strategies/how-to-setup-sso-integrations/azureadv2.md)
  * [Connect Apps to Microsoft AD FS](strategies/how-to-setup-sso-integrations/adfs.md)
  * [Connect Apps to Azure AD B2C](strategies/how-to-setup-sso-integrations/azureadb2c.md)
  * [Connect Apps to WeChat](strategies/how-to-setup-sso-integrations/wechat.md)
* [Biometric login](strategies/biometric.md)
* [Anonymous Users](strategies/anonymous-users.md)
* [Passwordless Login for Apple App Store Review](strategies/passwordless-demo-user-for-apple-app-review.md)
* [Login with Ethereum & NFT](strategies/web3.md)

## Integrate

* [Using SDK to call your application server](integrate/using-sdk-to-call-your-application-server.md)
* [User Settings](integrate/auth-ui.md)
* [User Profile](integrate/user-profile.md)
* [Reauthentication](integrate/reauthentication.md)
* [How Authgear integrate with your applications](integrate/how-authgear-integrate-with-your-applications.md)
* [Single Sign-on](integrate/single-sign-on.md)
* [Force authentication on app launch](integrate/force-authentication-on-app-launch.md)
* [Account Deletion](integrate/account-deletion.md)
* [Using Authgear as an OpenID Connect Provider](integrate/oidc-provider.md)
* [Events and Hooks](integrate/events-hooks/README.md)
  * [Event List](integrate/events-hooks/event-list.md)
  * [Webhooks](integrate/events-hooks/webhooks.md)
  * [JavaScript / TypeScript Hooks](integrate/events-hooks/denohooks.md)

## Customize

* [Privacy Policy & Terms of Service Links](customize/privacy-policy-terms-of-service.md)
* [Branding in Auth UI](customize/branding.md)
* [Custom domain](customize/custom-domain.md)
* [Custom Email Provider](customize/custom-email-provider.md)
* [Customer Support Link](customize/customer-support-link.md)
* [Localization & UI Text](customize/localization.md)

## Attack Protection

* [Brute-force Protection](attack-protection/brute-force-protection.md)

## APIs

* [API for Client Applications](apis/api-for-client-applications.md)
* [Admin API](apis/admin-api/README.md)
  * [Authentication and Security](apis/admin-api/authentication-and-security.md)
  * [API Schema](apis/admin-api/api-schema.md)
  * [Using global node IDs](apis/admin-api/node-id.md)
  * [API Examples](apis/admin-api/api-examples/README.md)
    * [Search for users](apis/admin-api/api-examples/search-for-users.md)
    * [Update user's standard attributes](apis/admin-api/api-examples/update-users-standard-attributes.md)
    * [Update user's picture](apis/admin-api/api-examples/update-users-picture.md)
    * [Generate OTP code](apis/admin-api/api-examples/generate-otp-code.md)

## Monitor

* [Audit Log](monitor/audit-log.md)

## Client App SDKs

* [Javascript SDK Reference](https://authgear.github.io/authgear-sdk-js/docs/)
* [iOS SDK Reference](https://authgear.github.io/authgear-sdk-ios/)
* [Android SDK Reference](https://authgear.github.io/authgear-sdk-android/)
* [Flutter SDK Reference](https://authgear.github.io/authgear-sdk-flutter/)
* [Xamarin SDK Reference](https://authgear.github.io/authgear-sdk-xamarin/)

## Deploy on your Cloud

* [Running locally with Docker](deploy-on-your-cloud/local.md)
* [Deploy with Helm chart](deploy-on-your-cloud/helm.md)
* [Authenticating HTTP request with Nginx](deploy-on-your-cloud/auth-nginx.md)
* [Configurations](deploy-on-your-cloud/configurations/README.md)
  * [Environment Variables](deploy-on-your-cloud/configurations/env.md)
  * [authgear.yaml](deploy-on-your-cloud/configurations/authgear.yaml.md)
  * [authgear.secrets.yaml](deploy-on-your-cloud/configurations/authgear.secrets.yaml.md)

## Security Concerns

* [Non-HTTP scheme redirect URI](security-concerns/redirect-uri.md)
