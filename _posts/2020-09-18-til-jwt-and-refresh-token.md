---
layout: post
title: "[TIL] Why need refresh token?"
date: "2020-09-18"
categories:
  - TIL
excerpt: |
  Why do we need refresh token even when using JWT? Why access/refresh token strategy is safer?
feature_text: |
  ## Why do we need refresh token even when using JWT? Why access/refresh token strategy is safer?
  Using refresh token with access token may burdens server but enhances security.
feature_image: "https://images.unsplash.com/photo-1584985429926-08867327d3a6?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=934&q=80"
image: "https://images.unsplash.com/photo-1584985429926-08867327d3a6?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=934&q=80"
---

When I tried to implement Authentication in my web app, I was advised to use JWT based access token and refresh token.

I searched about refresh token and I encountered these two questions:<br />
**Why should I use refresh token? <br />
Why is it safer?**

I'm not sure the answer that I found is sufficient, yet I am pretty convinced the necessity of refresh token.

## Access Token & Refresh Token
The purpose of access token is to maintain session(keep authenticated), to fetch and modify protected data in server/DB.

- When the user is authenticated, the server issues access token and refresh token.
- access token is composed of JWT, and does not need to be stored in the server(stateless).
- When the user tries to access to auth-required resources, access token must be presented.
- When the access token has expired, the server responds with error, and the client sends refresh token to refresh access token.
- Access token has short life than refresh token.
For instance, access token could have 15 minutes of lifetime whereas refresh token could have 2 weeks of lifetime.
Generally 2 weeks to 1 month is generally recommended lifetime for refresh token.

## Necessity of Refresh Token
Consider the case that there is no refresh token:
1. If the lifespan of access token is short enough, the user need to authenticate quite often(depends on the lifespan of access token).
1. If the lifespan of access token is long enough, the vulnerability agumented proportionally to the max age of the access token.

They say it is safer to have short lifespan of access token with long lifespan of refresh token.
But why? Why is it safer? Does refresh token actually have any difference?

If the refresh token is leaked, it may be used to issue new access tokens until it exires or blocked.
Most of the advices recommends to store refresh token in a secure location/storage, yet the problem is, **there is no secure storage in the browser.** Local storage, session storage, cookie... those are all that web front-end developers have as options, yet neither of them are being considered as secure storage.

In this point of view, **refresh token may have no difference, compared to the access token that have same long lifespan just like the refresh token.**

So, why do we really need to implement refresh token?

## Enhancing Security with Refresh Token
Refresh Token itself does not guarantee security.
Only when proper security strategy are adopted, refresh token enhances safety.

On way, could be storing refresh token in secure server/DB.
This requires disk I/O and server-side resources.

**If the auth server stores refresh token with certain user data(geolocation, IP, device, browser etc), server could restrict malicious atempt to refresh the access token.**

For instance, **if you try to refresh your access token in different state or country, the server could block your atempt.**

In this case, the server need to store certain user information accompanied with the refresh token.
It costs some. Note that only when the access token expires, the token refresh process is initiated. 

It is relatively uncommon compared to access token usage.

So, when the user has been authenticated, the user could access to the protected data by sending access token, and since access token does not requires disk I/O, it is relatively faster.

Every 15 minutes or so, when the access token expires, the user may need to wait a little longer to refresh the access token and access to the protected data; yet it's much safer, since the refresh token strategy ensures safety with several other data to verify.

## Reference
- [The OAuth 2.0 Authorization Framework: Bearer Token Usage](https://tools.ietf.org/html/rfc6750#section-5)
- [Understanding Refresh Tokens](https://auth0.com/learn/refresh-tokens/)
- [Refresh Tokens: When to Use Them and How They Interact with JWTs](https://auth0.com/blog/refresh-tokens-what-are-they-and-when-to-use-them/)
- [Refresh Token과 Sliding Sessions를 활용한 JWT의 보안 전략](https://blog.ull.im/engineering/2019/02/07/jwt-strategy.html)
- [How to secure a refresh token?](https://stackoverflow.com/questions/55486341/how-to-secure-a-refresh-token)
