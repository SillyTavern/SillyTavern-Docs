---
icon: rss
order: 24
---
# Remote connections

Most often this is for people who want to use SillyTavern on their mobile phones while their PC runs the ST server on the same wifi network.

However, it can be used to allow remote connections from anywhere as well.

**IMPORTANT: SillyTavern is a single-user program, so anyone who logs in will be able to see all characters and chats, and be able to change any settings inside the UI.**

### 1. Managing whitelisted IPs

* Create a new text file inside your SillyTavern base install folder called `whitelist.txt`.
* Open the file in a text editor, add a list of IPs you want to be allowed to connect.
  * Each IP must be on its own line.
  * **127.0.0.1 MUST be included in the list, or you will not be able to connect on the host machine**
  * Indidivual IPs, and wildcard (*) IP ranges are accepted.
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

*Note: `config.conf` also has a `whitelist` array, which you can use in the same way, but this array will be ignored if `whitelist.txt` exists. We do not reccomend using the config.conf IP list, because using whitelist.txt is easier*

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

We do not recommend doing this, but you can open `config.conf` and change `whitelist` to `false`.

You must remove (or rename) `whitelist.txt` in the SillyTavern base install folder, if it exists.

This is usually an insecure practice, so we require you to set a username and password when you do this.

The username and password are set in `config.conf`.

After restarting your ST server, any device will be able to connect to it, regardless of their IP as long as they know the username and password.

### Still Unable To Connect?

* Create an inbound/outbound firewall rule for the port found in `config.conf`. Do NOT mistake this for portforwarding on your router, otherwise someone could find your chat logs and that's a big no-no.
* Enable the Private Network profile type in Settings > Network and Internet > Ethernet. This is VERY important for Windows 11, otherwise you would be unable to connect even with the aforementioned firewall rules.
