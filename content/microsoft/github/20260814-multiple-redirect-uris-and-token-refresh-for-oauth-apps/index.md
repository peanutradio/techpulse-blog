---
categories:
- MS
- GitHub
date: '2026-08-14T22:43:44+00:00'
description: 'We&rsquo;ve released multiple updates to the OAuth app and GitHub App
  platforms to support more secure app development:


  OAuth apps can opt in to expiring acces'
draft: false
original_url: https://github.blog/changelog/2026-08-14-multiple-redirect-uris-and-token-refresh-for-oauth-apps
source: GitHub Changelog
tags:
- Improvement
- client apps
title: Multiple redirect URIs and token refresh for OAuth apps
---

We&rsquo;ve released multiple updates to the OAuth app and GitHub App platforms to support more secure app development:

OAuth apps can opt in to expiring access tokens and refresh tokens.
OAuth apps can have multiple redirect URIs.
Both GitHub Apps and OAuth apps can enable wildcard matching for redirect URIs if needed.  

Rotating tokens for OAuth apps
OAuth apps can now request a short-lived token during the user authentication flow. If an app opts in, they get an access token that lives for eight hours and a refresh token that&rsquo;s valid for six months. When the access token expires, the app uses the refresh token to get a new token pair.
Developers can add refresh token support to their app in two ways:

Include the offline_access scope in their authentication request, which triggers the short-lived token pattern. This is how developers should test and roll out this change in their app.
Set their app registration to always use short-lived tokens. This can be used to force old clients to update and ensure that all clients are getting short-lived tokens.

Short-lived tokens are enabled by default for all new applications. If your authentication SDK doesn&rsquo;t support the refresh token flow, you can disable this while updating the SDK.

For more details on how to use expiring tokens with an OAuth app, see the &ldquo;Authorizing OAuth apps&rdquo; documentation.
Multiple redirect URI support for OAuth apps
OAuth apps can now register up to 10 redirect URIs (called &ldquo;callback URIs&rdquo; on GitHub), making it easier to support multiple environments, domains, or deployment configurations without creating separate apps.
Developers will find a new Add redirect URI button in their application settings, which can be used to add additional URLs to match against.

Wildcard matching for redirect URIs
OAuth apps and GitHub Apps can now enable wildcard matching for each redirect URI configured. This can allow redirects to multiple related sites (e.g., tenanted subdomains of your app) without registering a redirect URI per tenant.
When enabled, wildcard matching allows an authorization code (and user) to be sent from GitHub to any URL that matches a subdomain or additional path off of the redirect URI.
Wildcard matching can be abused if the site being redirected to does not have strong control over its routes (e.g., if it hosts user content). Review your app architecture before enabling this.

  Apps with only one redirect URI have wildcard matching enabled. This is a legacy behavior of GitHub that is now visible and controllable. Please review your apps and disable wildcard matching if you do not need it. This applies to all OAuth apps and any GitHub App that had a single redirect URI registered.


These improvements will all be included in GitHub Enterprise Server 3.23.
For more information about creating and managing your app&rsquo;s redirects safely, see user authorization callback URLs for GitHub Apps and creating an OAuth app.
Join the discussion in the GitHub Community.

The post Multiple redirect URIs and token refresh for OAuth apps appeared first on The GitHub Blog.

---
*원문: [https://github.blog/changelog/2026-08-14-multiple-redirect-uris-and-token-refresh-for-oauth-apps](https://github.blog/changelog/2026-08-14-multiple-redirect-uris-and-token-refresh-for-oauth-apps)*
