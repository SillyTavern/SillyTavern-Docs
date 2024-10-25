---
icon: rss
order: -30
---
# Remote connections

Most often this is for people who want to use SillyTavern on their mobile phones while their PC runs the ST server within the same WiFi network.

It is also the first step for allowing remote connections from outside the local network.

!!! warning
You should not use port forwarding to expose your ST server to the internet. Instead, use a VPN or a tunneling service like Cloudflare Zero Trust, ngrok, or Tailscale. See the [VPN and Tunneling](tunneling.md) guide for more information.
!!!

!!! danger Disclaimer
**NEVER HOST ANY INSTANCES TO THE OPEN INTERNET WITHOUT ENSURING PROPER SECURITY MEASURES FIRST.**

**WE ARE NOT RESPONSIBLE FOR ANY DAMAGE OR LOSSES IN CASES OF UNAUTHORIZED ACCESS DUE TO IMPROPER OR INADEQUATE SECURITY IMPLEMENTATION.**
!!!

## Allowing remote connections

By default, ST server only accepts local connections. In order to allow remote connections, set the parameter `listen` within `config.yaml` to `true`:
```yaml
# Listen for incoming connections
listen: true
```

## Access control

After activating the remote connection listening, you must turn on at least one access control method, or ST server will refuse to start.

### Access control by whitelist

* Create a new text file inside your SillyTavern base install folder called `whitelist.txt`.
* Open the file in a text editor, add a list of IPs you want to be allowed to connect.
  * Each IP must be on its own line.
  * **127.0.0.1 MUST be included in the list, or you will not be able to connect on the host machine**
  * Individual IPs, and wildcard (*) IP ranges are accepted.
  * CIDR masks are also accepted (eg. 10.0.0.0/24).

Examples:

```txt
192.168.0.1
192.168.0.20

//or simply

192.168.*.*
```

(the above wildcard IP range will allow any device on the local network to connect)

#### General Purpose whitelist.txt

Copy and paste this exactly:

```txt
192.168.*.*
127.0.0.1
```

This will allow any device on the same network as the host machine, as well as the host machine itself, to connect to ST.

* Save the `whitelist.txt` file.
* **Restart your SillyTavern server.**

*Note: `config.yaml` also has a `whitelist` array, which you can use in the same way, but this array will be ignored if `whitelist.txt` exists. We do not recommend using the `config.yaml` IP list, because using `whitelist.txt` is easier*

!!! Removing access control by whitelist
Change `whitelistMode` to `false`, remove (or rename) `whitelist.txt` in the SillyTavern base install folder, and restart the server.
!!!

### Access control by HTTP Basic Authentication

The server will ask for username and password whenever a client connects via HTTP. **This only works if the Remote connections (listen: true) are enabled.**

To enable HTTP BA, Open `config.yaml` in the SillyTavern base directory and search for `basicAuthMode` Set basicAuthMode to true and set username and password. Note: `config.yaml` will only exist if ST has been executed before at least once.
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

### Troubleshooting

Still unable to connect?

* Enable the Private Network profile type in Settings > Network and Internet > Ethernet. This is VERY important for Windows 11, otherwise, you would be unable to connect even with the aforementioned firewall rules.
* On Windows, the application may be blocked by the application firewall. The quickest way to fix this is to uninstall and reinstall node.js, and when prompted by the firewall, allow it to access the network. Otherwise, you will need to manually allow the node.js application through the Windows application firewall.
* On Linux, you may need to allow the port through the firewall. The command to do this is `sudo ufw allow 8000`. This will allow traffic on port 8000.

Do not modify the port forwarding settings on your router. This is not necessary for accessing ST within your local network, and can expose your server to the internet.

## HTTPS

### Start SillyTavern with TLS/SSL

To encrypt traffic from and to your ST instance, start the server with the `--ssl` flag.

Example:
```
node server.js --ssl
```
As per default, ST will search for your certificates inside the `certs` folder. If your files are located elsewhere, you can use the `--keyPath` and `--certPath` arguments.

Example:
```
node server.js --ssl --keyPath /home/user/certificates/privkey.pem --certPath /home/user/certificates/cert.pem
```

The user you're running SillyTavern with requires read permissions on the certificate files.

### How to get a certificate

The simplest, quickest way to get a certificate is by using [certbot](https://letsencrypt.org/getting-started/).
