---
order: 25
icon: gear
expanded: true
route: /administration/
---

# Administration

!!!warning
Despite following many security best practices, the SillyTavern server is not secure enough for exposure to the public internet.

**NEVER HOST ANY INSTANCES ON THE OPEN INTERNET WITHOUT ENSURING PROPER SECURITY MEASURES FIRST.**

**WE ARE NOT RESPONSIBLE FOR ANY DAMAGE OR LOSSES RESULTING FROM UNAUTHORIZED ACCESS DUE TO IMPROPER OR INADEQUATE SECURITY IMPLEMENTATION.**
!!!

:::callout
**[config.yaml](./config-yaml.md)**

The main configuration file for SillyTavern. It contains various settings, such as network, security, and backend-specific options.
:::

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

Enthusiasts can set up a reverse proxy to access their SillyTavern instance from the internet.
:::

## Security checklist

**These are just recommendations. Please consult a web application security specialist before making your ST instance live.**

1. Keep your operating system and runtime software, such as Node.js, up to date. This ensures your system has the latest security patches and fixes, which helps prevent potential vulnerabilities.
2. Use a [whitelist](/Administration/config-yaml.md#ip-whitelisting) and a network firewall. Only allow trusted IP ranges to access the server.
3. Enable [basic authentication](/Administration/config-yaml.md#user-authentication). It acts as a "master password" before you can access the front-end app.
4. Alternatively, configure external authentication. Some known services for this are [Authelia](https://www.authelia.com/) and [authentik](https://goauthentik.io/). See the [SSO guide](sso.md) for details.
5. Never leave admin accounts without passwords. The server will warn you on startup if you have any unprotected admin accounts.
6. Use the discreet login setting outside the local network. This hides the user list from potential outsiders.
7. Check the access logs often. They are written to the server console and to the `access.log` file and provide information about incoming connections, such as IP address and user agent.
8. Configure HTTPS. For a localhost server, you can generate and use a self-signed certificate. Otherwise, you may need to deploy a reverse-proxy web server such as [Traefik](https://traefik.io/) or [Caddy](https://caddyserver.com/docs/getting-started).
9. Configure and enable [host whitelisting](/Administration/config-yaml.md#host-whitelisting), especially if you're not using HTTPS encryption on a local network.

Find more information about secure proxying in the following guide: [Reverse Proxying SillyTavern](reverse-proxying).
