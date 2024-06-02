---
icon: rss
order: 24
---
# Remote connections

Most often this is for people who want to use SillyTavern on their mobile phones while their PC runs the ST server within the same wifi network.

However, it can be used to allow remote connections from anywhere as well.

By default, ST server only accepts local connections. In order to allow remote connections, set the parameter `listen` within `config.yaml` to `true`:
```yaml
# Listen for incoming connections
listen: true
```
After activating the remote connection listening, you **MUST** turn on at least one of the restriction methods listed below as well, or ST server will refuse to start up.

**NOTE:** SillyTavern is a single-user program, so anyone who logs in will be able to see all characters and chats, and be able to change any settings inside the UI. [Basic Authentication](#HTTPBasicAuthentication) allows for requesting authorization before access to ST is granted, however the single-user principle still applies.

## Tailscale

This is the easiest way to remotely and safely use Tailscale from anywhere with any device. Anyone can do it and it's free. This is also an easy way to share your powerful PC with your friends. If you don't think Tailscale is a trustworthy company you can also host your own server using [Headscale](https://github.com/juanfont/headscale), but that is outside of the scope of this tutorial.

### 1. Creating an account

* Go to [Tailscale's website](https://tailscale.com/) and create a new account.

**NOTE:** For everyday use by a single person Tailscale will be permanently free. If you fear hidden costs, just don't add any payment options.

### 2. Setting up clients

* Go to [Tailscale's download page](https://tailscale.com/download) and download the client/app on the device you have SillyTavern running on and on the device you want to use it from remote location.
* Log in on both devices with your previously created account.
* Go to [Tailscale's admin page](https://login.tailscale.com/admin/machines) and approve both devices.
* Take note of both of the connected devices' names.

### 3. Adding your devices to the whitelist

* Now you can add your connecting device's machine name (the one you want to use SillyTavern with) to SillyTavern's whitelist by following [Managing whitelisted IPs](#managing-whitelisted-ips).

**NOTE:** Assuming you have not opened any ports for web traffic in your router settings, using the whitelist or basic auth is not necessary and you can disable both safely if you want since SillyTavern never gets publicly exposed when using Tailscale. If you disable basic auth and/or whitelisting you have to set `securityOverride` to true in *config.yaml* for SillyTavern to start.

### 4. Connecting

Now whenever you want to use SillyTavern from anywhere all you have to do is
* Have Tailscale turned on on both the PC hosting SillyTavern and your device that wants to use it remotely.
* Open a browser on the device that wants to connect and go to `http://<machine name of PC running st>:8000/`

### 5. Sharing SillyTavern instance with friend (optional)

* Tell your friend to create their own Tailscale account and download the client on their device.
* Go to [Tailscale's admin page](https://login.tailscale.com/admin/machines).
* Hover over the three-dot button on your PC hosting SillyTavern and press "Share..." or press the three-dot button and press "Sharing settings...".
* Uncheck "Allow use as an exit node" (unless you want your friend to be able to route all their internet traffic through your PC).
* Either send the link as an email or change tab to "Copy share link", press the big blue button with same text and send it to your friend in any other way. 
* After clicking your share link, your friend will see your PC pop up in their Tailscale network.
* Send your friend the same link you use to access SillyTavern as explained in the last step.

**NOTE:** This will give your friend full access to any services running locally on your PC like SillyTavern, automatic1111 etc. Only do this if you truly trust your friend.

## Whitelist

### 1. Managing whitelisted IPs {#managing-whitelisted-ips}

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

### 2. Getting the IP for the ST host machine

After the whitelist has been setup, you'll need the IP of the ST-hosting device.

If the ST-hosting device is on the same wifi network, you will use the ST-host's internal wifi IP:

* For Windows: windows button > type `cmd.exe` in the search bar > type `ipconfig` in the console, hit Enter > look for `IPv4` listing.

If you (or someone else) wants to connect to your hosted ST while not being on the same network, you will need the public IP of your ST-hosting device.

* While using the ST-hosting device, access [this page](https://whatismyipaddress.com/) and look for for `IPv4`. This is what you would use to connect from the remote device.

### 3. Connect the remote device to the ST host machine

Whatever IP you ended up with for your situation, you will put that IP address and port number into the remote device's web browser.

A typical address for an ST host on the same wifi network would look like:

`http://192.168.0.5:8000`

Use http:// NOT https://

### Opening your ST to all IPs

We do not recommend doing this, but you can open `config.yaml` and change `whitelistMode` to `false`.

You must remove (or rename) `whitelist.txt` in the SillyTavern base install folder, if it exists.

This is usually an insecure practice, so we require you to set a username and password when you do this.

The username and password are set in `config.yaml`.

After restarting your ST server, any device will be able to connect to it, regardless of their IP as long as they know the username and password.

### Still Unable To Connect?

* Create an inbound/outbound firewall rule for the port found in `config.yaml`. Do NOT mistake this for port forwarding on your router, otherwise, someone could find your chat logs and that's a big no-no.
* Enable the Private Network profile type in Settings > Network and Internet > Ethernet. This is VERY important for Windows 11, otherwise, you would be unable to connect even with the aforementioned firewall rules.

## <span id="HTTPBasicAuthentication">HTTP Basic Authentication</span>

The server will ask for username and password whenever a client connects via HTTP. **This only works if the Remote connections (listen: true) are enabled.**

To enable HTTP BA, Open `config.yaml` in the SillyTavern base directory and search for `basicAuthMode` Set basicAuthMode to true and set username and password. Note: `config.yaml` will only exist if ST has been executed before at least once.
```yaml
basicAuthMode: true
basicAuthUser:
  username: "MyUsername"
  password: "MyPassword"
```

Save the file and restart SillyTavern if it was already running. You should be prompted for username and password when connecting to your ST. Both username and password are transmitted in plain text. If you are concerned about this, you can serve ST via HTTPS.

## HTTPS

### Start SillyTavern with TLS/SSL

To encrypt traffic from and to your ST instance, start the server with the `--ssl` flag.

Example:
```
node server.js --ssl
```
As per default, ST will search for your certificates inside the /cert folder. If your files are located elsewhere, you can use the `--keyPath` and `--certPath` arguments.

Example:
```
node server.js --ssl --keyPath /home/user/certificates/privkey.pem --certPath /home/user/certificates/cert.pem
```

The user you're running SillyTavern with requires read permissions on the certificate files.

### How to get a certificate

The simplest, quickest way to get a certificate is by using [certbot](https://letsencrypt.org/getting-started/).
