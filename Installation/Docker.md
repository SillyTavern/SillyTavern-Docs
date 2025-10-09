---
# icon: container
label: Docker
---

# Docker Installation

!!!
These instructions assume you have installed Docker, are able to access your command line for the installation of containers, and familiar with their general operation.
!!!

## Using the GitHub Container Registry

Using a prebuilt image is the quickest and easiest way to get started with SillyTavern in Docker. You can pull the latest image from the GitHub Container Registry.

### Docker Compose (recommended)

Download the `docker-compose.yml` file from the [GitHub Repository](https://github.com/SillyTavern/SillyTavern/blob/release/docker/docker-compose.yml) and run the following command in the directory where the file is located. This will pull the latest release image from the GitHub Container Registry and start the container, automatically creating the necessary volumes.

```sh
docker compose up
```

You can edit the file and apply additional customization to suit your needs:

- The default port is 8000. You can change it by modifying the `ports` section.
- Change the `image` tag to `staging` if you want to use the development branch instead of the stable release.
- If you want to adjust the server configuration using environment variables, check the [Environment Variables](/Administration/config-yaml.md#environment-variables) page.

### Docker CLI (advanced)

You will need two mandatory directory mappings and a port mapping to allow SillyTavern to function. In the command, replace your selections in the following places:

#### Container Variables

##### Volume Mappings

- `CONFIG_PATH` - The directory where SillyTavern configuration files will be stored on your host machine
- `DATA_PATH` - The directory where SillyTavern user data (including characters) will be stored on your host machine
- `PLUGINS_PATH` - (optional) The directory where SillyTavern server plugins will be stored on your host machine
- `EXTENSIONS_PATH` - (optional) The directory where global UI extensions will be stored on your host machine

##### Port Mappings

- `PUBLIC_PORT` - The port to expose the traffic on. This is mandatory, as you will be accessing the instance from outside of its virtual machine container. DO NOT expose this to the internet without implementing a separate service for security.

##### Additional Settings

- `SILLYTAVERN_VERSION` - On the right-hand side of this GitHub page, you'll see "Packages". Select the "sillytavern" package and you'll see the image versions. The image tag "latest" will keep you up-to-date with the current release. You can also utilize "staging" that points to the nightly image of the respective branch.

#### Running the container

1. Open your Command Line
2. Run the following command in a folder where you want to store the configuration and data files:

```bash
SILLYTAVERN_VERSION="latest"
PUBLIC_PORT="8000"
CONFIG_PATH="./config"
DATA_PATH="./data"
PLUGINS_PATH="./plugins"
EXTENSIONS_PATH="./extensions"

docker run \
  --name="sillytavern" \
  -p "$PUBLIC_PORT:8000/tcp" \
  -v "$CONFIG_PATH:/home/node/app/config:rw" \
  -v "$DATA_PATH:/home/node/app/data:rw" \
  -v "$EXTENSIONS_PATH:/home/node/app/public/scripts/extensions/third-party:rw" \
  -v "$PLUGINS_PATH:/home/node/app/plugins:rw" \
  ghcr.io/sillytavern/sillytavern:"$SILLYTAVERN_VERSION"
```

!!!tip
By default the container will run in the foreground. If you want to run it in the background, add the `-d` flag to the `docker run` command.
!!!

## Building the Docker Image

!!!info
The following section assumes you installed SillyTavern in a non-root (non-admin) folder. If you installed SillyTavern in a root folder, you may have to run some of these commands with administrator rights [`sudo`, `doas`, Command Prompt (Administrator)].
!!!

If you want to build the Docker image yourself, you can do so by following these steps. This is useful if you want to customize the image or use it for development purposes.

### Linux

1. Install Docker by following the Docker installation guide [here](https://docs.docker.com/engine/install/).
   !!!danger
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

6. Open an new browser and go to [http://localhost:8000](http://localhost:8000). You should see SillyTavern load in a few moments.

### Windows

!!!warning Regarding Docker on Windows
Using Docker on Windows is **_really_** complicated. Not only do you need to activate _Windows Subsystem for Linux_ within _Turn Windows features on or off_, but also configure your system for Virtualization (Intel VT-d/AMD SVM) which differs from PC manufacturer to PC manufacturer (or motherboard manufacturer). Sometimes, this option is not present on some systems.

It is highly suggested you install SillyTavern by following our [Windows](/Installation/Windows.md) guide. This section is a _rough_ idea of how it can be done on Windows.
!!!

1.  Install Docker Desktop by following the Docker installation guide [here](https://docs.docker.com/desktop/setup/install/windows-install/).
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

5.  Open an new browser and go to [http://localhost:8000](http://localhost:8000). You should see SillyTavern load in a few moments.

### macOS

!!!
Even though macOS is similar to Linux, it doesn't have the Docker Engine. You will have to install Docker Desktop similarly to Windows.
You will also need to install [Homebrew](https://brew.sh/) in order to install Git on your Mac. This section is a _rough_ idea on how it can be done on macOS.
!!!

1.  Install Docker Desktop by following the Docker installation guide [here](https://docs.docker.com/desktop/setup/install/mac-install/).
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

    !!!info
    Note that if you configured Docker network as a bridge, you could also add external IP addresses to the whitelist as usual.
    !!!

7.  Restart the Docker Container to apply the new configuration.

    ```sh
    docker compose restart sillytavern
    ```

8.  Open an new browser and go to [http://localhost:8000](http://localhost:8000). You should see SillyTavern load in a few moments.

## Configuring SillyTavern

SillyTavern's configuration file (config.yaml) will be located within the `config` folder. Configuring the config file should be no different than configuring it without Docker, however you will need to run `nano` or a code editor with administrator rights in order to save your changes.

!!!warning
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

## Common issues with Docker

### SELinux Permission Issues with Mounted Volumes

Linux distributions with SELinux enabled (such as RHEL, CentOS, Fedora, etc.) may prevent Docker containers from accessing mounted volumes due to security policies. This can result in permission denied errors when the container tries to read or write to the mounted directories.

Two suffixes `:z` or `:Z` can be added to the volume mount. These suffixes tell Docker to relabel file objects on the shared volumes.

- The `z` option is used when the volume content will be shared between containers.
- The `Z` option is used when the volume content should only be used by the current container.

Example:

```yaml
# docker-compose.yml
volumes:
  ## Shared volume
  - ./config:/home/node/app/config:z
  ## Private volume
  - ./data:/home/node/app/data:Z
```

## Forbidden by Whitelist

!!!
Docker gateway IPs should be whitelisted automatically if [whitelistDockerHosts](/Administration/config-yaml.md#ip-whitelisting) config value is set to `true`.

If you are still unable to access SillyTavern, follow the instructions below to update the whitelist manually. 
!!!

1. Execute the following Docker command to obtain the IP of your SillyTavern Docker container.

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

2. For Windows, running Notepad or a code editor of your choice with administrator rights, go to config and open config.yaml. For Linux/macOS, using `sudo`, open `nano` and run the following command.

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

    !!!info
    Note that if you configured Docker network as a bridge, you could also add external IP addresses to the whitelist as usual.
    !!!

3. Restart the Docker Container to apply the new configuration.

    ```sh
    docker compose restart sillytavern
    ```
