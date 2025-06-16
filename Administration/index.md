---
order: 25
icon: gear
expanded: true
---

# Administration

!!!warning
Despite following many security best practices, the SillyTavern server is not secure enough for public internet exposure.

**NEVER HOST ANY INSTANCES TO THE OPEN INTERNET WITHOUT ENSURING PROPER SECURITY MEASURES FIRST.**

**WE ARE NOT RESPONSIBLE FOR ANY DAMAGE OR LOSSES IN CASES OF UNAUTHORIZED ACCESS DUE TO IMPROPER OR INADEQUATE SECURITY IMPLEMENTATION.**
!!!

:::callout
**[Multi-user](multi-user)**

To share your SillyTavern instance with others, you can create multiple user accounts. Each user has their own settings, extensions, and data. User accounts can also be password-protected.
:::

:::callout
**[Remote access](remote-connections)**

You can access your SillyTavern instance from your phone, tablet, or another computer.
:::


:::callout
**[VPNs and Tunneling](tunneling.md)**

To access your SillyTavern instance from the internet, you can use a VPN or a tunneling service like Cloudflare Zero Trust, ngrok, or Tailscale.
:::

:::callout
**[Reverse proxying](reverse-proxying)**

For enthusiasts, you can set up a reverse proxy to access your SillyTavern instance from the internet.
:::



## Security checklist

**This is just a recommendation. Please consult a web application security specialist before making your ST instance live.**

1. Keep your operating system and runtime software like Node.js updated. This will ensure that your system is up-to-date with the latest security patches and fixes which can help prevent potential vulnerabilities.
2. Use a whitelist and a network firewall. Only allow trusted IP ranges to access the server.
3. Enable basic authentication. It acts as a "master password" before you can proceed to the front-end app.
4. Alternatively, configure external authentication. Some known services for that are [Authelia](https://www.authelia.com/) and [authentik](https://goauthentik.io/). See more in the [SSO guide](sso.md).
5. Never leave admin accounts passwordless. A server will warn you upon the startup if you have any unprotected admin accounts.
6. Use the discreet login setting outside of the local network. This will hide the user list from any potential outsiders.
7. Check the access logs often. They are written to the server console and the `access.log` file and provide information on incoming connections, such as IP address and user agent.
8. Configure HTTPS. For a localhost server, you can generate and use a self-signed certificate. Otherwise, you may need to deploy a proxying web server like [Traefik](https://traefik.io/) or [Caddy](https://caddyserver.com/docs/getting-started).

Find more on secure proxying in the following guide: [Reverse Proxying SillyTavern](reverse-proxying).
