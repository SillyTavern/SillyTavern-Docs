---
label: Reverse Proxying SillyTavern
order: 9
icon: server
---

!!! danger Note
This section does **not** refer to OpenAI/Claude reverse proxies. This refers exclusively to **HTTP/HTTPS Reverse Proxies**.
!!!

Is Termux confusing to setup? Are you tired of updating and installing ST on every device you have? Want organization of your chats and characters? Well you are in luck. This guide will _hopefully_ cover how to host SillyTavern on your PC where you can connect from anywhere and chat to your bots on the same PC you use to run AI models!

!!! warning Warning
This guide is **not meant** for beginners. This will be very technical.
!!!

## Fair Warning

!!! info For Windows Users
Refer to [Windows](#windows) under _[Installation](#installation)_.
!!!

!!! info For Linux Users
You must have prior knowledge of

-   Linux console commands
-   DNS Records
-   Public IP addresses
-   [Docker](https://docker.com)

!!!

**You will have to buy a domain for yourself and configure a `CNAME` for your SillyTavern page. We suggest adding or buying the domain on [Cloudflare](https://cloudflare.com) as this guide will cover how to do this with Cloudflare itself.**

## Installation

### Linux (Bare-Metal SillyTavern)

For Linux, we will reverse proxying SillyTavern through [Traefik](https://traefik.io/traefik/). There are other options such as _NGINX_ or _Caddy_, but for this guide, we will use Traefik as it is what we use ourselves.

1. Get the private IP of your computer using `ifconfig` or from your router.
   !!! info Tip
   It is recommended to set your private IP to a Static IP. Refer to your router's manual or Google to configure static IPs.
   !!!
2. Get your public IP of your modem by Googling `what's my ip`.
   !!! info About Public IPs
   Most residential/home networks use **Dynamic IPs** which are renewed after months of use. If you have a dynamic IP, use either DDClient or remember to check and change your public IP ever so often on the Cloudflare Dashboard.
   !!!
3. Install Docker by following the Docker installation guide [here](https://docs.docker.com/engine/install/).
   !!! danger Note
   **Do not** install Docker Desktop.
   !!!
4. Follow the steps in **Manage Docker as a non-root user** in the Docker post-installation guide [here](https://docs.docker.com/engine/install/linux-postinstall/).
5. Go to your root folder in Linux and make a new folder named `docker`.
    ```sh
    cd /
    sudo mkdir docker && cd docker
    ```
6. Execute `chown`, replacing _<USER>_ with your Linux username to set the permissions in the docker folder.
    ```sh
    sudo chown -R <USER>:<USER> .
    ```
7. Make a folder inside the _docker_ folder, that being `secrets` and inside _secrets_ being `cloudflare`.
    ```sh
    mkdir secrets && mkdir secrets/cloudflare
    ```
8. Make a folder inside the _docker_ folder, that being `appdata` and inside _appdata_ being `traefik`. Enter the `appdata/traefik` folder afterwards.
    ```sh
    mkdir appdata && mkdir appdata/traefik
    cd appdata/traefik
    ```
9. Create a _acme.json_ file using `touch` and set the permissions of it to 600.
    ```sh
    touch acme.json
    chmod 600 acme.json
    ```
10. Using `nano` or a similar editor, create a file name _traefik.yml_ and paste the following. Replace the template email with your own, then save the file.
    ```yml
    api:
        dashboard: true
        debug: true
        insecure: true
    entryPoints:
        http:
            address: ":80"
            http:
                redirections:
                    entryPoint:
                        to: https
                        scheme: https
        https:
            address: ":443"
    serversTransport:
        insecureSkipVerify: true
    providers:
        docker:
            endpoint: "unix:///var/run/docker.sock"
            exposedByDefault: false
        file:
            filename: /config.yml
            watch: true
    certificatesResolvers:
        cloudflare:
            acme:
                email: YOUR_CLOUDFLARE_EMAL@DOMAIN.com
                storage: acme.json
                dnsChallenge:
                    provider: cloudflare
                    #disablePropagationCheck: true  # uncomment this if you have issues pulling certificates through cloudflare, By setting this flag to true disables the need to wait for the propagation of the TXT record to all authoritative name servers.
                    resolvers:
                        - "1.1.1.1:53"
                        - "1.0.0.1:53"
    ```
11. Return back to the `docker` folder.
    ```sh
    cd /docker
    ```
12. Using `nano` or a similar editor, create a file name _docker-compose.yaml_ and paste the following. Save the file afterwards.

    ```yaml
    secrets:
        CF_DNS_API_KEY:
            file: ./secrets/cloudflare/CF_DNS_API_KEY

    services:
        traefik:
            image: traefik:latest
            container_name: traefik
            restart: unless-stopped
            secrets:
                - CF_DNS_API_KEY
            ports:
                - 80:80
                - 443:443
                - 8080:8080
            environment:
                CLOUDFLARE_DNS_API_TOKEN_FILE: /run/secrets/CF_DNS_API_KEY
                CLOUDFLARE_ZONE_API_TOKEN_FILE: /run/secrets/CF_DNS_API_KEY
            volumes:
                - /var/run/docker.sock:/var/run/docker.sock:ro
                - ./appdata/traefik/traefik.yml:/traefik.yml:ro
                - ./appdata/traefik/config.yml:/config.yml:ro
                - ./appdata/traefik/acme.json:/acme.json
                - /etc/localtime:/etc/localtime:ro

    networks:
        internal:
            driver: bridge
    ```

13. Login to Cloudflare and click on your Domain, followed by **Get your API token**.
14. Click on _Create Token_ then _Create Custom Token_ and make sure you give your token the following permissions.
    !!! info Token Permissions
    **Zone -> DNS -> Edit**

    **Zone -> Zone -> Read**
    !!!

    Click on _Continue to summary_ followed by _Create Token._

15. Copy the Token Key given to you and store it somewhere secure.
16. `cd` into `secrets/cloudflare` and using `nano` or a similar editor, create a file named **CF_DNS_API_KEY** and paste your key inside.
17. Return to your domain page and go to **DNS**. Create a new record using **Add record** and create two _A_ type keys like the ones below. Replace `PUBLIC_IP` with your own public IP, then click _Save_.

    | Type | Name (required) | Target (required) | Proxy Status | TTL  |
    | ---- | --------------- | ----------------- | ------------ | ---- |
    | A    | DOMAIN.com      | PUBLIC_IP         | Proxied      | Auto |
    | A    | www             | PUBLIC_IP         | Proxied      | Auto |

18. Create another record of the **`CNAME`** type, then click _Save_. Here is an example on how it should appear on the Cloudflare dashboard.

    | Type  | Name (required) | Target (required) | Proxy Status | TTL |
    | ----- | --------------- | ----------------- | ------------ | --- |
    | CNAME | silly           | DOMAIN.com        | Proxied      | N/A |

19. `cd` into _appdata/traefik_ and using `nano` or a similar editor, create a file name _config.yml_ and paste the following. Replace `PRIVATE_IP` with the private IP you obtained, and `silly.DOMAIN.com` with the name of your subdomain and domain page, then save the file.

    ```yml
    http:
        routers:
            sillytavern:
                entryPoints:
                    - "https"
                rule: "Host(`silly.DOMAIN.com`)"
                middlewares:
                    - https-redirectscheme
                tls: {}
                service: sillytavern

        services:
            sillytavern:
                loadBalancer:
                    servers:
                        - url: "http://PRIVATE_IP:8000"
                    passHostHeader: true

        middlewares:
            https-redirectscheme:
                redirectScheme:
                    scheme: https
                    permanent: true
    ```

20. Run Docker Compose using the following commands:
    ```sh
    cd /docker
    docker compose up -d
    ```
21. Go to your SillyTavern folder and edit `config.yaml` to enable listen mode and basic authentication, whilst disabling `whitelistMode`.

    ```yaml
    listen: yes
    whitelistMode: false
    basicAuthMode: true
    ```

    !!! warning Tip
    Make sure to change the default username and password to something strong that you can remember.
    !!!

!!! warning
This toggle described below is currently only available in the `staging` branch. 
!!!

Or to use the SillyTavern accounts as usernames and passwords:

```yaml
basicAuthMode: true
enableUserAccounts: true
perUserBasicAuth: true
```

!!! warning Tip
Before enabling perUserBasicAuth ensure you have a valid multi-user setup with working passwords.
!!!

22. Wait a few minutes, then open your domain page you made for ST. At the end of it, you should be able to open SillyTavern from anywhere you go just with one URL and one account.
    !!! info Tip
    If nothing happens after several minutes, check the container logs for Traefik for any possible errors.
    !!!
23. Enjoy! :D

### Linux (Docker SillyTavern)

!!! warning Note
Do note that we run SillyTavern on bare-metal over Docker. This is a rough idea of what we would do on Docker with other Docker containers we tend to use with ST.
!!!

1. Follow Steps 1-11 of **Linux (Bare-Metal SillyTavern)**.
2. Login to Cloudflare and click on your Domain, followed by **Get your API token**.
3. Click on _Create Token_ then _Create Custom Token_ and make sure you give your token the following permissions.
   !!! info Token Permissions
   **Zone -> DNS -> Edit**

    **Zone -> Zone -> Read**
    !!!

    Click on _Continue to summary_ followed by _Create Token._

4. Copy the Token Key given to you and store it somewhere secure.
5. `cd` into `secrets/cloudflare` and using `nano` or a similar editor, create a file named **CF_DNS_API_KEY** and paste your key inside.
6. Return to your domain page and go to **DNS**. Create a new record using **Add record** and create two _A_ type keys like the ones below. Replace `PUBLIC_IP` with your own public IP and the example domain with your domain, then click _Save_.

    | Type | Name (required) | Target (required) | Proxy Status | TTL  |
    | ---- | --------------- | ----------------- | ------------ | ---- |
    | A    | DOMAIN.com      | PUBLIC_IP         | Proxied      | Auto |
    | A    | www             | PUBLIC_IP         | Proxied      | Auto |

7. Create another record of the **`CNAME`** type, then click _Save_. Here is an example on how it should appear on the Cloudflare dashboard.

    | Type  | Name (required) | Target (required) | Proxy Status | TTL |
    | ----- | --------------- | ----------------- | ------------ | --- |
    | CNAME | silly           | DOMAIN.com        | Proxied      | N/A |

8. Git clone SillyTavern into the `docker` folder.
    ```sh
    cd /docker && git clone https://github.com/SillyTavern/SillyTavern
    ```
9. Using `nano` or a similar editor, create a file name _docker-compose.yaml_ and paste the following. Replace `silly.DOMAIN.com` with the subdomain you added above, the save the file afterwards.

    ```yaml
    secrets:
        CF_DNS_API_KEY:
            file: ./secrets/cloudflare/CF_DNS_API_KEY

    services:
        traefik:
            image: traefik:latest
            container_name: traefik
            restart: unless-stopped
            secrets:
                - CF_DNS_API_KEY
            ports:
                - 80:80
                - 443:443
                - 8080:8080
            environment:
                CLOUDFLARE_DNS_API_TOKEN_FILE: /run/secrets/CF_DNS_API_KEY
                CLOUDFLARE_ZONE_API_TOKEN_FILE: /run/secrets/CF_DNS_API_KEY
            volumes:
                - /var/run/docker.sock:/var/run/docker.sock:ro
                - ./appdata/traefik/traefik.yml:/traefik.yml:ro
                - ./appdata/traefik/config.yml:/config.yml:ro
                - ./appdata/traefik/acme.json:/acme.json
                - /etc/localtime:/etc/localtime:ro
        sillytavern:
            build: ./SillyTavern
            container_name: sillytavern
            hostname: sillytavern
            image: ghcr.io/sillytavern/sillytavern:latest
            volumes:
                - "./appdata/sillytavern/config:/home/node/app/config"
                - "./appdata/sillytavern/data:/home/node/app/data"
            restart: unless-stopped
            labels:
                - "traefik.enable=true"
                - "traefik.http.routers.sillytavern.entrypoints=http"
                - "traefik.http.routers.sillytavern.rule=Host(`silly.DOMAIN.com`)"
                - "traefik.http.middlewares.sillytavern-https-redirect.redirectscheme.scheme=https"
                - "traefik.http.routers.sillytavern.middlewares=sillytavern-https-redirect"
                - "traefik.http.routers.sillytavern-secure.entrypoints=https"
                - "traefik.http.routers.sillytavern-secure.rule=Host(`silly.DOMAIN.com`)"
                - "traefik.http.routers.sillytavern-secure.tls=true"
                - "traefik.http.routers.sillytavern-secure.service=sillytavern"
                - "traefik.http.services.sillytavern.loadbalancer.server.port=8000"

    networks:
        internal:
            driver: bridge
    ```

10. Run Docker Compose using the following commands:
    ```sh
    docker compose up -d
    ```
11. Stop the SillyTavern Docker container.
    ```sh
    docker compose stop sillytavern
    ```
12. Go to your SillyTavern folder (`appdata/sillytavern/config`) and edit `config.yaml` to enable listen mode and basic authentication, whilst disabling `whitelistMode`.

    ```yaml
    listen: yes
    whitelistMode: false
    basicAuthMode: true
    ```

    !!! warning Tip
    Make sure to change the default username and password to something strong that you can remember.
    !!!

13. Start the SillyTavern Docker container again.
    ```sh
    docker compose up -d sillytavern
    ```
14. Wait a few minutes, then open your domain page you made for ST. At the end of it, you should be able to open SillyTavern from anywhere you go just with one URL and one account.
    !!! info Tip
    If nothing happens after several minutes, check the container logs for Traefik for any possible errors.
    !!!
15. Enjoy! :D

### Windows

With Windows, reverse proxying is a bit different. Currently your only options available for Windows (from what we know) is by running a VPN on your router or a Cloudflare/NGROK Tunnel. Refer to [**Other Options**](#other-options) for more information.

!!! info Fact of the Day
_Did you know that Windows eats more VRAM than Linux? We suggest dual-booting Windows and Linux or go pure Linux to save some VRAM if you are needing more for context or higher parameter models._
!!!

## Optional Things

While we won't necessarily cover these things in much detail, here are some things we suggest trying to use if you want to secure your SillyTavern instance better. (Stuff will be added when it fits)

1. Use [**Authelia**](https://www.authelia.com/) or [**Authentik**](https://goauthentik.io/).

Authelia/Authentik is a open-source single sign-on (SSO) app that allows you to create users and secure many different pages using a login portal presented on sites you want to secure. One of us primarily use this over ST's authentication for their own domain use and while it is complex to setup, it is a good way to both learn SSO and secure your ST instance out on the internet more.

If you plan to use Authelia to secure ST's, you must enable `securityOverride` in *config.yaml*. Authlia will handle the login security, but ST has no way of knowing this and will refuse to start without securityOverride.

If using Authlia it is recommened to disable ST's basic auth since Authlia provides better functionality and the same security.

!!! warning
This toggle described below is currently only available in the `staging` branch. 
!!!

If your Authelia username exactly matches the username of a SillyTavern user account in multi-user mode you can enable `autheliaAuth` in the *config.yaml* to directly login to the account using Authelia.

2. Use [**DDClient**](https://ddclient.net/).

    DDClient allows you to sync your public IP to Cloudflare in the situation that your ISP changes it, allowing you to continue accessing your ST instance as if nothing ever happened.

## Other Options

If reverse proxying is a bit much for you, there are still other ways to connect to your ST instance.

1. Use a **home-made VPN**.

    Several routers come with the ability to host a VPN server (primarily OpenVPN or WireGuard) in the router administration page. Refer to your router's manual to setup a VPN and add your devices to the VPN. Once connected, just go to the private IP you have set for SillyTavern and you can connect just fine. Easier for users and for Windows use.

2. Use [**Cloudflare Zero Trust**](https://developers.cloudflare.com/cloudflare-one/).

    Cloudflare Zero Trust is a free organizational feature in Cloudflare that allows you to add 50 users. This will proxy your traffic through Cloudflare and by adding your ST PC as a tunnel using `cloudflared`, you can connect to your ST instance as if you were home.

    Do note that after making a tunnel, you will have to add a route to your router's private IP addresses and calculate IP CIDR values to have full local access on the go using Cloudflare Zero Trust.

3. Use a standalone **Cloudflare/NGROK Tunnel**.

    Similar to how AI backends can connect, you can also connect your ST instance via a Cloudflare Tunnel and open the Cloudflare Tunnel page. However, you will have to copy and paste each new link generated by Cloudflare/NGROK each time you want to use ST on-the-go.
