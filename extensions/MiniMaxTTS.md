---
order: XTTS-MiniMax
---

# MiniMax TTS

This page will teach you how to properly use the MiniMax TTS provider.

## Prerequisites

1. MiniMax account with API access
2. Valid API Key and Group ID from MiniMax

## Getting API Credentials

### 1. Create a MiniMax Account

1. Visit the [MiniMax website](https://www.minimax.io/)
2. Click "Sign Up" or "Login"
3. Complete the account registration process

!!!warning Regional Differences
MiniMax has separate Chinese and International versions. Please note:
- The Chinese version does not support voice cloning features
- The Chinese version only supports the `api.minimax.chat` API host
!!!

### 2. Obtain API Key

1. Log into the [MiniMax console](https://www.minimax.io/platform/user-center/basic-information)
2. You can find your GroupId on the Basic Information page
3. Go to Settings → API Keys on the left sidebar to create and obtain your API Key

## Configuration in SillyTavern

### 1. Basic Setup

1. Open SillyTavern
2. Navigate to "Extensions" → "TTS"
3. Select "MiniMax" as your TTS provider
4. Configure the following settings:
    - **API Key**: Your MiniMax API key
    - **Group ID**: Your MiniMax Group ID
    - **API Host**: Choose the appropriate server based on your region:
        - `api.minimax.io` (Official server)
        - `api.minimaxi.chat` (Global server)
        - `api.minimax.chat` (China mainland server)

### 2. Model Selection

Available models include:
- **Speech-02-HD**: High-quality voice synthesis (recommended)
- **Speech-02-Turbo**: Fast voice synthesis
- **Speech-01**: Legacy model
- **Speech-01-240228**: Legacy model (specific version)

### 3. Voice Parameters

Adjust the following parameters to customize voice output:
- **Speed**: 0.5 - 2.0 (1.0 = normal speed)
- **Volume**: 0.1 - 2.0 (1.0 = normal volume)
- **Pitch**: 0.5 - 2.0 (1.0 = normal pitch)
- **Audio Format**: MP3, WAV, FLAC

## Custom Voices

### 1. Adding Custom Voices

1. In the MiniMax TTS settings, locate the "Custom Voice Management" section
2. Fill in the following information:
    - **Voice Name**: Choose any name for identification
    - **Voice ID**: The voice ID obtained from the MiniMax platform
    - **Language**: Select the corresponding language for the voice
3. Click "Add Custom Voice"

### 2. Obtaining Voice IDs

1. Access the MiniMax TTS interface
2. Click "Voice" on the right side to enter the Voice Selection interface
3. Find the voice you want to use
4. Click the copy button next to the voice name to copy the Voice ID

## Custom Models

### 1. Adding Custom Models

1. In the "Custom Model Management" section
2. Fill in:
    - **Model ID**: Model identifier
    - **Model Name**: Display name for the model
3. Click "Add Custom Model"

### 2. Obtaining Model IDs

1. Check the model list in the official MiniMax documentation
2. Or view available custom models in the console
3. Copy the corresponding Model ID

## Troubleshooting

### Common Issues

1. **API Authentication Failed**
    - Verify that the API Key corresponds to the correct API Host
    - Confirm that the Group ID is correct
    - Check if your account has sufficient balance

2. **Voice Generation Failed**
    - Verify that the selected Voice ID is valid
    - Ensure the voice is compatible with your selected model

3. **Connection Timeout**
    - Try switching to a different API Host
    - Check your network connection
    - Verify firewall settings

4. **Audio Quality Issues**
    - Try using a different model (Speech-02-HD for best quality)
    - Adjust voice parameters (speed, pitch, volume)
    - Check audio format compatibility
