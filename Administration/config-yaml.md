---
icon: cpu
route: /administration/config-yaml/
---

# Configuration File

!!!warning Disclaimer

This documentation may be obsolete, incomplete, or incorrect. Please refer to the [default config.yaml](https://github.com/SillyTavern/SillyTavern/blob/release/default/config.yaml) in your installation for the most up-to-date list of settings.

**WARNING: DO NOT EDIT THE DEFAULT CONFIG DIRECTLY. THIS WON'T HAVE ANY POSITIVE EFFECT. EDIT ITS COPY IN THE REPOSITORY ROOT INSTEAD.**
!!!

`config.yaml` is the main configuration file for the SillyTavern server which you can find in the repository root directory after [completing the installation](/Installation/index.md). It is a YAML file that contains various settings, such as network, security, and backend-specific options. **The changes made to this file will take effect after restarting the server.**

New settings that are added upstream are automatically populated with default values when you run `npm install` (specifically, the `post-install.js` script) after [updating the repository](/Installation/Updating/index.md). You can then modify these settings as needed.

For nested settings, dot notation is used to indicate the hierarchy. For example, `protocol.ipv6: false` refers to the `ipv6` setting under the `protocol` section with a value of `false`.

```yaml
protocol:
  ipv6: false
```

## Command-Line Arguments

You can pass command-line arguments when starting the SillyTavern server to override some settings in [config.yaml](../Administration/config-yaml.md).

### Examples

```shell
node server.js --port 8000 --listen false
# or
npm run start -- --port 8000 --listen false
# or (Windows only)
Start.bat --port 8000 --listen false
```

### Supported arguments

!!!tip
None of the arguments are required. If you don't provide them, SillyTavern will use the settings in `config.yaml`.
!!!

| Option                          | Description                                                          | Type     |
|---------------------------------|----------------------------------------------------------------------|----------|
| `--version`                     | Shows the version number                                             | boolean  |
| `--global`                      | Forces the use of system-wide paths for application data             | boolean  |
| `--configPath`                  | Overrides the path to the config.yaml file (standalone mode only)    | string   |
| `--dataRoot`                    | Sets the root directory for data storage (standalone mode only)      | string   |
| `--port`                        | Sets the port under which SillyTavern will run                       | number   |
| `--listen`                      | Makes SillyTavern listen on all network interfaces                   | boolean  |
| `--whitelist`                   | Enables whitelist mode                                               | boolean  |
| `--basicAuthMode`               | Enables basic authentication                                         | boolean  |
| `--enableIPv4`                  | Enables the IPv4 protocol                                            | boolean  |
| `--enableIPv6`                  | Enables the IPv6 protocol                                            | boolean  |
| `--listenAddressIPv4`           | Specifies the IPv4 address to listen on                              | string   |
| `--listenAddressIPv6`           | Specifies the IPv6 address to listen on                              | string   |
| `--dnsPreferIPv6`               | Prefers IPv6 for DNS                                                 | boolean  |
| `--ssl`                         | Enables SSL                                                          | boolean  |
| `--certPath`                    | Sets the path to your certificate file                               | string   |
| `--keyPath`                     | Sets the path to your private key file                               | string   |
| `--browserLaunchEnabled`        | Automatically launches SillyTavern in the browser                    | boolean  |
| `--browserLaunchHostname`       | Sets the browser launch hostname                                     | string   |
| `--browserLaunchPort`           | Overrides the port for browser launch                                | string   |
| `--browserLaunchAvoidLocalhost` | Avoids using 'localhost' for browser launch in auto mode             | boolean  |
| `--corsProxy`                   | Enables the CORS proxy                                               | boolean  |
| `--requestProxyEnabled`         | Enables the use of a proxy for outgoing requests                     | boolean  |
| `--requestProxyUrl`             | Sets the request proxy URL (HTTP or SOCKS protocols)                 | string   |
| `--requestProxyBypass`          | Sets the request proxy bypass list (space-separated list of hosts)   | array    |
| `--disableCsrf`                 | Disables CSRF protection (NOT RECOMMENDED)                           | boolean  |

## Environment Variables

Configuration may also be set via environment variables, which will override the values in the `config.yaml` file.

The environment variables should be prefixed with `SILLYTAVERN_` and use uppercase letters for the setting names. For example, the `dataRoot` setting can be overridden with the `SILLYTAVERN_DATAROOT` environment variable.

The nested settings should be separated by underscores. For example, `protocol.ipv6` can be overridden with the `SILLYTAVERN_PROTOCOL_IPV6` environment variable.

!!!warning
Configurations that expect arrays or objects should be JSON-stringified. For example, to override the `whitelist` setting with the `SILLYTAVERN_WHITELIST` environment variable, you should set it as a JSON string: `SILLYTAVERN_WHITELIST='["127.0.0.1", "::1"]'`.
!!!

If you are using Node.js v20 or later, you can also store environment variables in a `.env` file and pass it to the server with the `--env-file` flag. For example, to use the `.env` file located in the repository root, you can start the server with the following command:

```bash
node --env-file=.env server.js
```

Alternatively, pass the environment variables directly via the command line:

```bash
SILLYTAVERN_LISTEN=true SILLYTAVERN_PORT=8000 node server.js
```

See more about using environment variables in the [Node.js documentation](https://nodejs.org/en/learn/command-line/how-to-read-environment-variables-from-nodejs).

## Data Configuration

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `dataRoot` | Root directory for user data storage (standalone mode only) | `./data` | Any valid directory path |
| `skipContentCheck` | Skip checks for new default content | `false` | `true`, `false` |
| `enableDownloadableTokenizers` | Enable on-demand tokenizer downloads | `true` | `true`, `false` |

## Logging Configuration

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|------------------|
| `logging.minLogLevel` | Minimum log level to display in the terminal | `0` (DEBUG) | (DEBUG = 0, INFO = 1, WARN = 2, ERROR = 3) |
| `logging.enableAccessLog` | Write server access logs to a file and the console | `true` | `true`, `false` |

## [Network Configuration](/Administration/remote-connections.md)

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `listen` | Enable listening for incoming connections | `false` | `true`, `false` |
| `port` | Server listening port | `8000` | Any valid port number (1-65535) |
| `protocol.ipv4` | Enable listening on the IPv4 protocol | `true` | `true`, `false`, `auto` |
| `protocol.ipv6` | Enable listening on the IPv6 protocol | `false` | `true`, `false`, `auto` |
| `listenAddress.ipv4` | Listen on a specific IPv4 address | `0.0.0.0` | Valid IPv4 address |
| `listenAddress.ipv6` | Listen on a specific IPv6 address | `'[::]'` | Valid IPv6 address |
| `dnsPreferIPv6` | Prefer IPv6 for DNS resolution | `false` | `true`, `false` |

## SSL Configuration

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|------------------|
| `ssl.enabled` | Enable SSL/TLS encryption and HTTPS protocol | `false` | `true`, `false` |
| `ssl.keyPath` | Path to SSL private key (relative to the server directory) | `"./certs/privkey.pem"` | Valid file path |
| `ssl.certPath` | Path to SSL certificate (relative to the server directory) | `"./certs/cert.pem"` | Valid file path |
| `ssl.keyPassphrase` | Passphrase for the SSL private key. Leave empty if not required | `""` | Any string |

## Security Configuration

### IP Whitelisting

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `whitelistMode` | Enable IP whitelist filtering | `true` | `true`, `false` |
| `enableForwardedWhitelist` | Check forwarded headers for whitelisted IPs | `true` | `true`, `false` |
| `whitelist` | List of allowed IP addresses | `["::1", "127.0.0.1"]` | Array of valid IP addresses |
| `whitelistDockerHosts` | Automatically whitelist Docker host IPs | `true` | `true`, `false` |

### Host Whitelisting

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `hostWhitelist.enabled` | Enable host whitelisting | `false` | `true`, `false` |
| `hostWhitelist.scan` | Log incoming requests from untrusted hosts | `true` | `true`, `false` |
| `hostWhitelist.hosts` | List of trusted hostnames | `[]` | Array of valid hostnames |

### Security Overrides

!!!danger
**DISABLING SECURITY MEASURES IS HIGHLY DISCOURAGED. PLEASE MAKE SURE YOU UNDERSTAND WHAT YOU ARE DOING BEFORE MAKING CHANGES.**
!!!

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|------------------|
| `allowKeysExposure` | Allow unmasked API key exposure in the UI | `false` | `true`, `false` |
| `disableCsrfProtection` | Disable CSRF protection (not recommended) | `false` | `true`, `false` |
| `securityOverride` | Disable startup security checks (not recommended) | `false` | `true`, `false` |

## [User Authentication](/Administration/multi-user.md)

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `basicAuthMode` | Enable basic authentication | `false` | `true`, `false` |
| `basicAuthUser.username` | Basic auth username | `"user"` | Any string |
| `basicAuthUser.password` | Basic auth password | `"password"` | Any string |
| `enableUserAccounts` | Enable multi-user mode | `false` | `true`, `false` |
| `enableDiscreetLogin` | Hide the user list on the login screen | `false` | `true`, `false` |
| `sessionTimeout` | User session timeout in seconds | `-1` (disabled) | Any number (-1 to disable, 0 for browser close, >0 for timeout) |
| `perUserBasicAuth` | Use account credentials for basic auth | `false` | `true`, `false` |

### SSO Auto-Login

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|------------------|
| `sso.autheliaAuth` | Enable Authelia-based auto-login. See: [SSO](/Administration/sso.md) | `false` | `true`, `false` |
| `sso.authentikAuth` | Enable Authentik-based auto-login. See: [SSO](/Administration/sso.md) | `false` | `true`, `false` |

## Rate Limiting Configuration

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|------------------|
| `rateLimiting.preferRealIpHeader` | Use the X-Real-IP header instead of the socket IP for rate limiting | `false` | `true`, `false` |

## Request Proxy Configuration

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `requestProxy.enabled` | Enable proxy for outgoing requests | `false` | `true`, `false` |
| `requestProxy.url` | Proxy server URL | `null` | Valid proxy URL (e.g., `"socks5://username:password@example.com:1080"`) |
| `requestProxy.bypass` | Hosts to bypass the proxy | `["localhost", "127.0.0.1"]` | Array of hostnames/IPs |

## CORS Proxy Configuration

!!!
An enabled CORS proxy may be required by some extensions. It is not required by any built-in features.
!!!

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `enableCorsProxy` | Enable CORS proxy middleware | `false` | `true`, `false` |

## Browser Launch Configuration

> Previously known as "Autorun" settings.

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `browserLaunch.enabled` | Open the browser automatically on server startup | `true` | `true`, `false` |
| `browserLaunch.browser` | Browser to use for opening the URL | `"default"` | `"default"`, `"chrome"`, `"firefox"`, `"edge"`, `"brave"` |
| `browserLaunch.hostname` | Override the hostname for browser launch | `"auto"` | `"auto"`, any valid hostname (e.g., `"localhost"`, `"st.example.com"`) |
| `browserLaunch.port` | Override the port for browser launch | `-1` | `-1` (use server port), any valid port number |
| `browserLaunch.avoidLocalhost` | Avoid using 'localhost' in the launch URL | `false` | `true`, `false` |

## Performance Configuration

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|------------------|
| `performance.lazyLoadCharacters` | Lazy-load character data | `true` | `true`, `false` |
| `performance.useDiskCache` | Enable disk caching for character cards | `true` | `true`, `false` |
| `performance.memoryCacheCapacity` | Maximum memory cache capacity | `100mb` | Human-readable size (e.g., `100mb`, `1gb`) |

## Cache Buster Configuration

!!!warning
Requires localhost or a domain with HTTPS, otherwise will not work!
!!!

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|------------------|
| `cacheBuster.enabled` | Clear browser cache on first load or after uploading image files | `false` | `true`, `false` |
| `cacheBuster.userAgentPattern` | Only clear the cache for user agents matching the specified regex pattern. Example: `'firefox'` (case-insensitive). | `''` | Any valid regex string |

## Thumbnailing Configuration

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `thumbnails.enabled` | Enable thumbnail generation | `true` | `true`, `false` |
| `thumbnails.quality` | JPEG thumbnail quality | `95` | 0-100 |
| `thumbnails.format` | Image format for thumbnails | `jpg` | `jpg`, `png` |
| `thumbnails.dimensions.bg` | Background thumbnails size | `[160, 90]` | Array of two numbers (width, height) |
| `thumbnails.dimensions.avatar` | Avatar thumbnails size | `[96, 144]` |  Array of two numbers (width, height) |
| `thumbnails.dimensions.persona` | Persona thumbnails size | `[96, 144]` |  Array of two numbers (width, height) |

## Backup Configuration

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `backups.chat.enabled` | Enable automatic chat backups | `true` | `true`, `false` |
| `backups.chat.checkIntegrity` | Verify integrity of chat files before saving | `true` | `true`, `false` |
| `backups.common.numberOfBackups` | Number of backups to keep | `50` | Any positive integer |
| `backups.chat.throttleInterval` | Backup throttle interval (ms) | `10000` | Any positive integer |
| `backups.chat.maxTotalBackups` | Maximum total chat backups to keep | `-1` | Any positive integer or -1 |

## [Extensions Configuration](/extensions/index.md)

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `extensions.enabled` | Enable UI extensions | `true` | `true`, `false` |
| `extensions.autoUpdate` | Auto-update extensions (if enabled by the extension manifest) | `true` | `true`, `false` |
| `extensions.models.autoDownload` | Enable automatic model downloads | `true` | `true`, `false` |
| `extensions.models.classification` | HuggingFace model ID for classification | `"Cohee/distilbert-base-uncased-go-emotions-onnx"` | Valid model ID |
| `extensions.models.captioning` | HuggingFace model ID for image captioning | `"Xenova/vit-gpt2-image-captioning"` | Valid model ID |
| `extensions.models.embedding` | HuggingFace model ID for embeddings | `"Cohee/jina-embeddings-v2-base-en"` | Valid model ID |
| `extensions.models.speechToText` | HuggingFace model ID for speech-to-text | `"Xenova/whisper-small"` | Valid model ID |
| `extensions.models.textToSpeech` | HuggingFace model ID for text-to-speech | `"Xenova/speecht5_tts"` | Valid model ID |

## [Server Plugins](/For_Contributors/Server-Plugins.md)

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `enableServerPlugins` | Enable server-side plugins | `false` | `true`, `false` |
| `enableServerPluginsAutoUpdate` | Attempt to automatically update server plugins on startup | `true` | `true`, `false` |

## [API Integration Settings](/Usage/API_Connections/index.md)

### OpenAI Configuration

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `promptPlaceholder` | Default message for empty prompts | `"[Start a new chat]"` | Any string |
| `openai.randomizeUserId` | Randomize the user ID for API calls | `false` | `true`, `false` |
| `openai.captionSystemPrompt` | System message for caption completion | `""` | Any string |

### MistralAI Configuration

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `mistral.enablePrefix` | Enable reply prefilling. **The prefix will be echoed in the response** | `false` | `true`, `false` |

### Ollama Configuration

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `ollama.keepAlive` | Model keep-alive duration (seconds) | `-1` | `-1` (indefinite), `0` (immediate unload), positive integer |
| `ollama.batchSize` | Controls the "num_batch" (batch size) parameter of the generation request | `-1` | `-1` (model default), positive integer |

### Claude Configuration

!!!warning **IMPORTANT!**

Use with caution and only when the prompt prefix is static and doesn't change between requests. \{\{random\}\} macro, lorebooks, vectors, summaries, etc. will likely invalidate the cache and you'll just waste money on cache misses. Behavior may be unpredictable and no guarantees can or will be made.

See: [Prompt Caching](https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching)
!!!

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `claude.enableSystemPromptCache` | Enable system prompt caching | `false` | `true`, `false` |
| `claude.cachingAtDepth` | Enable message history caching | `-1` | `-1` (disabled), `0` or positive integer |
| `claude.extendedTTL` | Use 1h TTL instead of the default 5m. Note that this also increases the cost of the request. | `false` | `true`, `false` |

### Google AI Studio Configuration

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `gemini.apiVersion` | API endpoint version | `v1beta` | `v1beta`, `v1alpha` |

### DeepL Configuration

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `deepl.formality` | Translation formality level | `"default"` | `"default"`, `"more"`, `"less"`, `"prefer_more"`, `"prefer_less"` |
