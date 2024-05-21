---
# icon: container
label: Docker
---

# Docker Installation

!!! info
This guide assumes you installed SillyTavern in a non-root (non-admin) folder. If you installed SillyTavern in a root folder, you may have to run some of these commands with administrator rights [`sudo`, `doas`, Command Prompt (Administrator)].
!!!

## Installation

### Linux

1. Install Docker by following the Docker installation guide [here](https://docs.docker.com/engine/install/).
   !!! danger
   **Do not** install Docker Desktop.
   !!!
2. Follow the steps in **Manage Docker as a non-root user** in the Docker [Post-Installation Guide](https://docs.docker.com/engine/install/linux-postinstall/).
3. Install [Git](https://git-scm.com/download/linux) using your package manager.

    - Debian (Ubuntu/Pop! OS/etc.)

        ```sh
        sudo apt install git
        ```

    - Arch Linux (Manjaro/EndeavourOS/etc.)

        ```sh
        sudo pacman -S git
        ```

    - Fedora, Red Hat Enterprise Linux (RHEL), etc.
        ```sh
        sudo dnf install git
        ```

4. Clone the SillyTavern repository.

    - Release (Stable Branch)

        ```sh
        git clone https://github.com/SillyTavern/SillyTavern && cd SillyTavern/docker
        ```

    - Staging (Development Branch)
        ```sh
        git clone https://github.com/SillyTavern/SillyTavern -b staging && cd SillyTavern/docker
        ```

5. Execute `docker compose` by running the following command within the Docker folder.

    ```sh
    docker compose up -d
    ```

6. Execute the following Docker command to obtain the IP of your SillyTavern Docker container.

    ```sh
    docker network inspect docker_default
    ```

    You should receive some sort of output similar to the following below.

    ```json
    [
        {
            "Name": "docker_default",
            "IPAM": {
                "Config": [
                    {
                        "Subnet": "172.18.0.0/16",
                        "Gateway": "172.18.0.1"
                    }
                ]
            }
        }
    ]
    ```

    Copy down the IP you see in _Gateway_ as this will be important.

7. Using `sudo`, open `nano` and run the following command.

    ```sh
    sudo nano config/config.yaml
    ```

    Within `nano`, go down to `whitelist`. You should see something similar to the following below.

    ```yaml
    whitelist:
        - 127.0.0.1
    ```

    Add a new line below _127.0.0.1_ and put in the IP you copied from Docker. It should look something similar to the following afterwards.

    ```yaml
    whitelist:
        - 127.0.0.1
        - 172.18.0.1
    ```

    Save the file by pressing _Ctrl+S_ then exit `nano` by pressing _Ctrl+X_.

!!! info
Note that if you configured Docker network as a bridge, you could also add external IP addresses to the whitelist as usual.
!!!

9. Restart the Docker Container to apply the new configuration.

    ```sh
    docker compose restart sillytavern
    ```

10. Open an new browser and go to [http://localhost:8000](http://localhost:8000). You should see SillyTavern load in a few moments.

11. Enjoy! :D

### Windows

!!! warning Regarding Docker on Windows
Using Docker on Windows is **_really_** complicated. Not only do you need to activate _Windows Subsystem for Linux_ within _Turn Windows features on or off_, but also configure your system for Virtualization (Intel VT-d/AMD SVM) which differs from PC manufacturer to PC manufacturer (or motherboard manufacturer). Sometimes, this option is not present on some systems.

It is highly suggested you install SillyTavern by following our [Windows](/Installation/Windows.md) guide. This section is a _rough_ idea of how it can be done on Windows.
!!!

1.  Install Docker Desktop by following the Docker installation guide [here](https://docs.docker.com/desktop/install/windows-install/).
2.  Install [Git for Windows](https://git-scm.com/download/win).
3.  Clone the SillyTavern repository.

    -   Release (Stable Branch)

        ```sh
        git clone https://github.com/SillyTavern/SillyTavern && cd SillyTavern/docker
        ```

    -   Staging (Development Branch)
        ```sh
        git clone https://github.com/SillyTavern/SillyTavern -b staging && cd SillyTavern/docker
        ```

4.  Execute `docker compose` by running the following command within the Docker folder.

    ```sh
    docker compose up -d
    ```

5.  Execute the following Docker command to obtain the IP of your SillyTavern Docker container.

    ```sh
    docker network inspect docker_default
    ```

    You should receive some sort of output similar to the following below.

    ```json
    [
        {
            "Name": "docker_default",
            "IPAM": {
                "Config": [
                    {
                        "Subnet": "172.18.0.0/16",
                        "Gateway": "172.18.0.1"
                    }
                ]
            }
        }
    ]
    ```

    Copy down the IP you see in _Gateway_ as this will be important.

6.  Running _Notepad_ or a code editor of your choice with administrator rights, go to `config` and open _config.yaml_.

    Within the editor of your choice, you should see something similar to the following below.

    ```yaml
    whitelist:
        - 127.0.0.1
    ```

    Add a new line below _127.0.0.1_ and put in the IP you copied from Docker. It should look something similar to the following afterwards.

    ```yaml
    whitelist:
        - 127.0.0.1
        - 172.18.0.1
    ```

    Save the file by pressing _Ctrl+S_ then exit your editor.

!!! info
Note that if you configured Docker network as a bridge, you could also add external IP addresses to the whitelist as usual.
!!!

7.  Restart the Docker Container to apply the new configuration.

    ```sh
    docker compose restart sillytavern
    ```

8.  Open an new browser and go to [http://localhost:8000](http://localhost:8000). You should see SillyTavern load in a few moments.

9.  Enjoy! :D

### macOS

!!!
Even though macOS is similar to Linux, it doesn't have the Docker Engine. You will have to install Docker Desktop similarly to Windows.
You will also need to install [Homebrew](https://brew.sh/) in order to install Git on your Mac. This section is a _rough_ idea on how it can be done on macOS.
!!!

1.  Install Docker Desktop by following the Docker installation guide [here](https://docs.docker.com/desktop/install/mac-install/).
2.  Install `git` using Homebrew.

    ```sh
    brew install git
    ```

3.  Clone the SillyTavern repository.

    -   Release (Stable Branch)

        ```sh
        git clone https://github.com/SillyTavern/SillyTavern && cd SillyTavern/docker
        ```

    -   Staging (Development Branch)
        ```sh
        git clone https://github.com/SillyTavern/SillyTavern -b staging && cd SillyTavern/docker
        ```

4.  Execute `docker compose` by running the following command within the Docker folder.

    ```sh
    docker compose up -d
    ```

5.  Execute the following Docker command to obtain the IP of your SillyTavern Docker container.

    ```sh
    docker network inspect docker_default
    ```

    You should recieve some sort of output similar to the following below.

    ```json
    [
        {
            "Name": "docker_default",
            "IPAM": {
                "Config": [
                    {
                        "Subnet": "172.18.0.0/16",
                        "Gateway": "172.18.0.1"
                    }
                ]
            }
        }
    ]
    ```

    Copy down the IP you see in _Gateway_ as this will be important.

6.  Using `sudo`, open `nano` and run the following command.

    ```sh
    sudo nano config/config.yaml
    ```

    !!!
    If you can't run `nano`, either install it via Homebrew or use TextEdit.
    !!!

    Within `nano`, go down to `whitelist`. You should see something similar to the following below.

    ```yaml
    whitelist:
        - 127.0.0.1
    ```

    Add a new line below _127.0.0.1_ and put in the IP you copied from Docker. It should look something similar to the following afterwards.

    ```yaml
    whitelist:
        - 127.0.0.1
        - 172.18.0.1
    ```

    Save the file by pressing _Ctrl+S_ then exit `nano` by pressing _Ctrl+X_.

!!! info
Note that if you configured Docker network as a bridge, you could also add external IP addresses to the whitelist as usual.
!!!

7.  Restart the Docker Container to apply the new configuration.

    ```sh
    docker compose restart sillytavern
    ```

8.  Open an new browser and go to [http://localhost:8000](http://localhost:8000). You should see SillyTavern load in a few moments.

9.  Enjoy! :D

## Configuring SillyTavern

SillyTavern's configuration file (config.yaml) will be located within the `config` folder. Configuring the config file should be no different than configuring it without Docker, however you will need to run `nano` or a code editor with administrator rights in order to save your changes.

!!! warning
Don't forget to restart the Docker container for SillyTavern in order to apply your changes! Make sure you execute this command within the `docker` folder.

```sh
docker compose restart sillytavern
```

!!!

## Locating User Data

SillyTavern's data folder will be within the `data` folder. Backing up your files should be easy to do, however, restoring or adding content into it may require you to do so with administrator rights.

## Running Server Plugins

Running plugins like [HoYoWiki-Scraper-TS](https://github.com/Bronya-Rand/HoYoWiki-Scraper-TS) or [SillyTavern-Fandom-Scraper](https://github.com/SillyTavern/SillyTavern-Fandom-Scraper) within Docker is no different from running it on your system without Docker, however we will need to do a slight modification to the Docker Compose script in order to do so.

!!! Note
If you already see a _plugins_ folder within the `docker` folder, you can skip Steps 1-2.
!!!

1. Using `nano` or a code editor, open _docker-compose.yml_ and add the following line below `volumes`.

```sh
    volumes:
        - "./config:/home/node/app/config"
        - "./data:/home/node/app/data"
        - "./plugins:/home/node/app/plugins"
```

2. Create a new folder within the `docker` folder called _plugins_.
3. Follow your plugin's instructions on installing the plugin.
4. Using `nano` or a code editor with administrator rights, open _config.yaml_ (within the `config` folder) and enable `enableServerPlugins`

```sh
enableServerPlugins: true
```

5. Restart the Docker container.

```sh
docker compose restart sillytavern
```

6. Profit.
