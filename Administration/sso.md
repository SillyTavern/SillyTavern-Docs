---
label: Single Sign-On (SSO)
icon: key
order: -20
---

# Single Sign-On (SSO)

Use [**Authelia**](https://www.authelia.com/) or [**Authentik**](https://goauthentik.io/).

Authelia and Authentik are open-source single sign-on (SSO) apps. SSO allows you to create users and secure many different pages using a login portal presented on sites you want to secure. While it is complex to setup, it is a good way to both learn SSO and secure your ST instance out on the internet more.

If you plan to use Authelia to secure ST's, you must enable `securityOverride` in *config.yaml*. Authlia will handle the login security, but ST has no way of knowing this and will refuse to start without securityOverride.

If using Authelia it is recommened to disable ST's basic auth since Authlia provides better functionality and the same security.

If your Authelia username exactly matches the username of a SillyTavern user account in multi-user mode you can enable `autheliaAuth` in the *config.yaml* to directly login to the account using Authelia.
