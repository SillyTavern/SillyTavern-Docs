---
icon: rss
order: -30
route: /usage/remoteconnections/
---

# Remote connections

Most often this is for people who want to use SillyTavern on their mobile phones while their PC runs the ST server within the same WiFi network.

It is also the first step for allowing remote connections from outside the local network.

!!!warning
You should not use port forwarding to expose your ST server to the internet. Instead, use a VPN or a tunneling service like Cloudflare Zero Trust, ngrok, or Tailscale. See the [VPN and Tunneling](tunneling.md) guide for more information.
!!!

!!!danger Disclaimer
**NEVER HOST ANY INSTANCES ON THE OPEN INTERNET WITHOUT ENSURING PROPER SECURITY MEASURES FIRST.**

**WE ARE NOT RESPONSIBLE FOR ANY DAMAGE OR LOSSES IN CASES OF UNAUTHORIZED ACCESS DUE TO IMPROPER OR INADEQUATE SECURITY IMPLEMENTATION.**
!!!

## Allowing remote connections

By default, the ST server only accepts connections from the machine that it's running on (localhost). To allow it to listen for connections from other devices, set the `listen` option in `config.yaml` to `true`.

!!! If you search for `config.yaml` directly in the SillyTavern folder, you may find two files.
All modifications to `config.yaml` in this document refer to the one in the SillyTavern root directory (/SillyTavern/config.yaml), not `/SillyTavern/default/config.yaml`.
!!!

```yaml
# Listen for incoming connections
listen: true
```

When ST is listening for remote connections, you should see this message in the console:

```txt
SillyTavern is listening on IPv4: 0.0.0.0:8000
```

and some explanation about what that means.

When ST is **not** listening for remote connections, you should see this message in the console:

```txt
SillyTavern is listening on IPv4: 127.0.0.1:8000
```

## Access control configuration

After enabling remote connection listening, you must configure at least one access control method. Otherwise, the server will not start.

### Whitelist-Based access control

To enable access control via a whitelist, edit the `config.yaml` file in the SillyTavern root directory (`/SillyTavern/config.yaml`):

1. Start SillyTavern at least once to generate the necessary configuration files.
2. Open `/SillyTavern/config.yaml` in a text editor.
3. Find the `whitelist` section and add the IP addresses you wish to allow:
    * List each IP address separately.
    * Ensure `127.0.0.1` is included, or you will be unable to connect from the host machine.
    * Supports individual IPs, CIDR masks (e.g., `10.0.0.0/24`), and wildcard (`*`) ranges.
4. Save the `config.yaml` file.
5. **Restart your SillyTavern server.**

#### Example `config.yaml` whitelist configuration

1. Allow any device on the local network:

    ```yaml
    whitelist:
      - ::1
      - 127.0.0.1
      - 10.0.0.0/8
      - 172.16.0.0/12
      - 192.168.0.0/16
    ```

    If unsure about your local network's address range, use the whitelist above.

2. Allows two specific devices to connect:

    ```yaml
    whitelist:
      - ::1
      - 127.0.0.1
      - 192.168.0.2
      - 192.168.0.5
    ```

3. Allows any device on the `192.168.0.*` subnet to connect:

    ```yaml
    whitelist:
      - ::1
      - 127.0.0.1
      - 192.168.0.*
    ```

4. Allow network connections for all IPv4 devices:

    ```yaml
    whitelist:
      - 0.0.0.0/0
    ```

### Disabling whitelist-based access control

To disable access control via a whitelist:

* Set `whitelistMode` to `false` in `/SillyTavern/config.yaml`.
* Remove or rename `whitelist.txt` (if it exists) in the SillyTavern base installation folder.
* Restart your SillyTavern server.

### Not recommended: using `whitelist.txt`

!!!info
If `whitelist.txt` exists, it takes precedence over the whitelist settings in `config.yaml`.

However, since all other configurations are managed within `config.yaml`, and `whitelist.txt` may encounter permission issues or become locked, the system could silently revert to using the `config.yaml` whitelist.

**Editing config.yaml directly is both simpler and more reliable.**
!!!

If you still prefer using whitelist.txt:

1. Create a new text file named `whitelist.txt` in the SillyTavern base installation folder.
2. Open it in a text editor and add the allowed IP addresses.
3. Save the file and restart your SillyTavern server.

#### Example `whitelist.txt` configuration

```txt
10.0.0.0/8
172.16.0.0/12
192.168.0.0/16
127.0.0.1
::1
```

This allows any device on the local network to connect.

### Access control by HTTP Basic Authentication

!!!warning
HTTP Basic Authentication does not provide strong security.

There is no rate-limiting to prevent brute-force attacks. If this is a concern, it is recommended to use a reverse proxy with TLS and rate-limiting, and a dedicated [authentication service](sso.md).
!!!

The server will ask for username and password whenever a client connects via HTTP. **This only works if the Remote connections (listen: true) are enabled.**

To enable HTTP BA, Open `config.yaml` in the SillyTavern base directory and search for `basicAuthMode`. Set basicAuthMode to true and set username and password. Note: `config.yaml` will only exist if ST has been executed before at least once.

```yaml
basicAuthMode: true
basicAuthUser:
  username: "MyUsername"
  password: "MyPassword"
```

Alternatively you can enable basic auth as follows:

```yaml
basicAuthMode: true
enableUserAccounts: true
perUserBasicAuth: true
```

In this `perUserBasicAuth` mode the basic auth's username and password will be the same as any valid multi user account that has a password. Additionally SillyTavern will login directly to that account. **Ensure you have an account with a password prior to enabling `perUserBasicAuth`.**

Save the file and restart SillyTavern if it was already running. You should be prompted for username and password when connecting to your ST. Both username and password are transmitted in plain text. If you are concerned about this, you can serve ST via HTTPS.

### Host whitelisting

When hosting a server over the network without HTTPS, it is highly recommended to enable request host verification. This helps prevent various attacks, such as DNS rebinding. By default, the SillyTavern server will log a console message on a first connection from an unrecognized host.

### Toggle host whitelisting

To enable host whitelisting, edit the `config.yaml` file in the SillyTavern root directory:

```yaml
hostWhitelist:
    enabled: true
```

### Add trusted hosts

To add a host name to a list of trusted hosts, include it in the `hostWhitelist.hosts` section:

!!!tip Tips
Do not add `localhost` or IPs (such as `127.0.0.1` or `::1`). These are always considered trusted.

To add a range of hosts, use a leading dot. For example, adding `.trycloudflare.com` will trust `trycloudflare.com` as well as any subdomain like `example.trycloudflare.com`.
!!!

```yaml
hostWhitelist:
  hosts:
    - "example.com"
    - ".trycloudflare.com"
```

### Toggle console messages

To disable console messages for unrecognized hosts, set the `hostWhitelist.scan` option to `false`:

```yaml
hostWhitelist:
    scan: false
```

## Connecting to your SillyTavern instance

### Getting the IP address for the ST host machine

After the whitelist has been setup, you'll need the IP of the ST-hosting device.

If the ST-hosting device is on the same wifi network, you will use the ST-host's internal wifi IP:

* For Windows: windows button > type `cmd.exe` in the search bar > type `ipconfig` in the console, hit Enter > look for `IPv4` listing.

If you (or someone else) wants to connect to your hosted ST while not being on the same network, you will need the public IP of your ST-hosting device.

* While using the ST-hosting device, access [this page](https://whatismyipaddress.com/) and look for for `IPv4`. This is what you would use to connect from the remote device.

### Connecting to the ST server

Whatever IP you ended up with for your situation, you will put that IP address and port number into the remote device's web browser.

A typical address for an ST host on the same wifi network would look like:

`http://192.168.0.5:8000`

Use http:// NOT https://

### Connection logging

New connections to the server are displayed in the console window and logged in the `access.log` file in the SillyTavern data directory.

A console message for a browser on the same machine as the server looks like:

```txt
New connection from 127.0.0.1; User Agent: ...
```

A console message for a browser on a different machine on the same network as the server might look like:

```txt
New connection from 192.168.116.187; User Agent: ...
```

If a connection is refused, the console message will look like:

```txt
New connection from 192.168.116.211; User Agent: ...

Forbidden: Connection attempt from 192.168.116.211. If you are attempting to connect, 
please add your IP address in whitelist or disable whitelist mode in config.yaml in 
root of SillyTavern folder.
```

`access.log` will contain the connection information, with timestamps, but not whether the connection was accepted or refused.

### Troubleshooting

Still unable to connect?

* If the connection attempt [appears in the console](#connection-logging), but is forbidden, it is a [whitelist issue](#whitelist-based-access-control).
* If ST is listening for remote connections but the connection attempt does not appear in the console, it is a [network issue](#network-issues).
* If ST is not listening for remote connections, it is a [reading issue](#allowing-remote-connections).

#### Network issues

* On Windows, the application may be blocked by the application firewall. The quickest way to fix this is to uninstall and reinstall node.js, and when prompted by the firewall, allow it to access the network. Otherwise, you will need to manually allow the node.js application through the Windows application firewall.
* On Windows 11, enable the Private Network profile type in Settings > Network and Internet > Ethernet. This is VERY important for Windows 11, otherwise, you would be unable to connect even with the aforementioned firewall rules.
* On Linux, you may need to allow the port through the firewall. The command to do this is `sudo ufw allow 8000`. This will allow traffic on port 8000.

Do not modify the port forwarding settings on your router. This is not necessary for accessing ST within your local network, and can expose your server to the internet.

If you are trying to access your ST server from [outside your local network](remote-connections.md), and it's not working, identify whether the problem is between the remote device and the tunnel/VPN endpoint, or between the tunnel endpoint on the server and the ST service. Otherwise you will spend a lot of time troubleshooting the wrong thing.

## HTTPS

### Start SillyTavern with TLS/SSL

!!!tip
SSL can also be configured using the `config.yaml` file: [SSL Configuration](/Administration/config-yaml.md#ssl-configuration).
!!!

To encrypt traffic from and to your ST instance, start the server with the `--ssl` flag.

Example:

```bash
node server.js --ssl
```

As per default, ST will search for your certificates inside the `certs` folder. If your files are located elsewhere, you can use the `--keyPath` and `--certPath` arguments.

Example:

```bash
node server.js --ssl --keyPath /home/user/certificates/privkey.pem --certPath /home/user/certificates/cert.pem
```

The user you're running SillyTavern with requires read permissions on the certificate files.

### How to get a certificate

The simplest, quickest way to get a certificate is by using [certbot](https://letsencrypt.org/getting-started/).

### Certificates in Docker

!!!warning
For security and privacy reasons, do not include your SSL certificates inside the Docker image if you are building one. Instead, use volume mounts to provide the certificates at runtime.
!!!

When running SillyTavern in Docker, the recommended way to provide SSL certificates is by placing them in the `/config` volume mount. This allows you to manage certificates without rebuilding the container image.

1. Place your certificate files (e.g., `privkey.pem` and `cert.pem`) in your local config directory that is mounted to `/config` in the container.

2. Update your `config.yaml` to reference the certificates:

    ```yaml
    ssl:
      enabled: true
      certPath: ./config/cert.pem
      keyPath: ./config/privkey.pem
    ```

3. Restart your Docker container to apply the changes.
