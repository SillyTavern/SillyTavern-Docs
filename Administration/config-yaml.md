# Server Configuration (config.yaml)

!!! warning Disclaimer

This documentation may be obsolete, incomplete, or incorrect. Please refer to the [default config.yaml](https://github.com/SillyTavern/SillyTavern/blob/release/default/config.yaml) in your installation for the most up-to-date list of settings.

!!!

`config.yaml` is the main configuration file for the SillyTavern server that you can find in the repository root directory after installing SillyTavern. It is a YAML file that contains various settings, such as the network settings, security settings, and backend-specific options. The changes made to this file will take effect after restarting the server.

New settings that added to the upstream version will be automatically populated with the default values when you run `npm install` (or specifically, the `post-install.js` script) after updating the repository. You can then modify these settings as needed.

For nested settings, dot notation is used to indicate the hierarchy. For example, `protocol.ipv6: false` refers to the `ipv6` setting under the `protocol` section with a value of `false`.

```yaml
protocol:
  ipv6: false
```

## Data Configuration

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `dataRoot` | Root directory for user data storage | `./data` | Any valid directory path |

## [Network Configuration](/Administration/remote-connections.md)

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `listen` | Enable listening for incoming connections | `false` | `true`, `false` |
| `port` | Server listening port | `8000` | Any valid port number (1-65535) |
| `protocol.ipv4` | Enable listening on IPv4 protocol | `true` | `true`, `false` |
| `protocol.ipv6` | Enable listening on IPv6 protocol | `false` | `true`, `false` |
| `dnsPreferIPv6` | Prefer IPv6 for DNS resolution | `false` | `true`, `false` |

## Security Configuration

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `whitelistMode` | Enable IP whitelist filtering | `true` | `true`, `false` |
| `enableForwardedWhitelist` | Check forwarded headers for whitelisted IPs | `true` | `true`, `false` |
| `whitelist` | List of allowed IP addresses | `["::1", "127.0.0.1"]` | Array of valid IP addresses |
| `enableCorsProxy` | Enable CORS proxy middleware | `false` | `true`, `false` |
| `allowKeysExposure` | Allow API keys exposure in the UI | `false` | `true`, `false` |
| `disableCsrfProtection` | Disable CSRF protection (not recommended) | `false` | `true`, `false` |
| `securityOverride` | Disable startup security checks (not recommended) | `false` | `true`, `false` |

## [User Authentication](/Administration/multi-user.md)

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `basicAuthMode` | Enable basic authentication | `false` | `true`, `false` |
| `basicAuthUser.username` | Basic auth username | `"user"` | Any string |
| `basicAuthUser.password` | Basic auth password | `"password"` | Any string |
| `enableUserAccounts` | Enable multi-user mode | `false` | `true`, `false` |
| `enableDiscreetLogin` | Hide user list on login screen | `false` | `true`, `false` |
| `sessionTimeout` | User session timeout in seconds | `86400` (24 hours) | Any number (-1 to disable, 0 for browser close, >0 for timeout) |
| `cookieSecret` | Secret for signing session cookies | `''` (auto-generated) | Any string |
| `autheliaAuth` | Enable Authelia-based auto login. See: [SSO](/Administration/sso.md) | `false` | `true`, `false` |
| `perUserBasicAuth` | Use account credentials for basic auth | `false` | `true`, `false` |

## Request Proxy Configuration

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `requestProxy.enabled` | Enable proxy for outgoing requests | `false` | `true`, `false` |
| `requestProxy.url` | Proxy server URL | `null` | Valid proxy URL (e.g., `"socks5://username:password@example.com:1080"`) |
| `requestProxy.bypass` | Hosts to bypass proxy | `["localhost", "127.0.0.1"]` | Array of hostnames/IPs |

## AutoRun Configuration

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `autorun` | Open browser automatically on startup | `true` | `true`, `false` |
| `autorunHostname` | Hostname used when autorun opens the browser | `"auto"` | `"auto"`, any valid hostname (e.g., `"localhost"`, `"st.example.com"`) |
| `autorunPortOverride` | Override port for browser autorun | `-1` | `-1` (use server port), any valid port number |
| `avoidLocalhost` | Avoid using 'localhost' for autorun | `false` | `true`, `false` |

## Advanced Configuration

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `disableThumbnails` | Disable thumbnail generation | `false` | `true`, `false` |
| `thumbnailsQuality` | JPEG thumbnail quality | `95` | 0-100 |
| `avatarThumbnailsPng` | Generate PNG thumbnails instead of JPEG | `false` | `true`, `false` |
| `skipContentCheck` | Skip new default content checks | `false` | `true`, `false` |
| `disableChatBackup` | Disable automatic chat backups | `false` | `true`, `false` |
| `numberOfBackups` | Number of backups to keep | `50` | Any positive integer |
| `chatBackupThrottleInterval` | Backup throttle interval (ms) | `10000` | Any positive integer |

## [Extensions Configuration](/extensions/index.md)

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `enableExtensions` | Enable UI extensions | `true` | `true`, `false` |
| `enableExtensionsAutoUpdate` | Auto-update extensions | `true` | `true`, `false` |
| `enableDownloadableTokenizers` | Enable on-demand tokenizer downloads | `true` | `true`, `false` |
| `extras.disableAutoDownload` | Disable automatic model downloads | `false` | `true`, `false` |
| `extras.classificationModel` | HuggingFace model ID for classification | `"Cohee/distilbert-base-uncased-go-emotions-onnx"` | Valid model ID |
| `extras.captioningModel` | HuggingFace model ID for image captioning | `"Xenova/vit-gpt2-image-captioning"` | Valid model ID |
| `extras.embeddingModel` | HuggingFace model ID for embeddings | `"Cohee/jina-embeddings-v2-base-en"` | Valid model ID |
| `extras.speechToTextModel` | HuggingFace model ID for speech-to-text | `"Xenova/whisper-small"` | Valid model ID |
| `extras.textToSpeechModel` | HuggingFace model ID for text-to-speech | `"Xenova/speecht5_tts"` | Valid model ID |

## [Server Plugins](/For_Contributors/Server-Plugins.md)

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `enableServerPlugins` | Enable server-side plugins | `false` | `true`, `false` |

## [API Integration Settings](/Usage/API_Connections/index.md)

### OpenAI Configuration

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `promptPlaceholder` | Default message for empty prompts | `"[Start a new chat]"` | Any string |
| `openai.randomizeUserId` | Randomize user ID for API calls | `false` | `true`, `false` |
| `openai.captionSystemPrompt` | System message for caption completion | `""` | Any string |

### MistralAI Configuration

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `mistral.enablePrefix` | Enable reply prefilling | `false` | `true`, `false` |

### Ollama Configuration

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `ollama.keepAlive` | Model keep-alive duration (seconds) | `-1` | `-1` (indefinite), `0` (immediate unload), positive integer |

### Claude Configuration

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `claude.enableSystemPromptCache` | Enable system prompt caching | `false` | `true`, `false` |
| `claude.cachingAtDepth` | Enable message history caching | `-1` | `-1` (disabled), `0` or positive integer |

### DeepL Configuration

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `deepl.formality` | Translation formality level | `"default"` | `"default"`, `"more"`, `"less"`, `"prefer_more"`, `"prefer_less"` |
