---
route: /usage/api-connections/google/
---

# Google Gemini

Gemini is Google's cutting-edge multimodal LLM, which is available though several APIs, including Google Vertex AI and Google AI Studio (formerly MakerSuite). This guide will help you set up the Gemini API connections in SillyTavern.

## Google AI Studio

AI Studio is the fastest and the most user-friendly way to try out the latest Google AI models without needing to set up a Google Cloud Platform (GCP) project. It provides a simple API key that you can use to access the Gemini models.

### Step 1: Create a Google AI Studio Key

1. Go to the [Google AI Studio](https://aistudio.google.com/apikey) page and sign in with your Google account.
2. Click on "Get API Key", accept the terms and conditions.
3. Click "Create API Key" to generate your API key.
4. Copy the API key to your clipboard.

### Step 2: Put the API Key into SillyTavern

1. In SillyTavern, go to the "API Connections" page.
2. Select "Chat Completion" as the API type.
3. Select "Google AI Studio" from the dropdown menu.
4. Enter the API key you copied earlier into the "API Key" text box.
5. Click the "Connect" button to save the key.

You should now be able to use the Google AI Studio API with SillyTavern.

## Google Vertex AI

Vertex AI is a service provided by Google Cloud Platform (GCP). It provides access to various AI models, including the Gemini series.

There are several ways a Vertex AI API can be set up, and the available models may vary depending on the method used.

### Service Account

Google Cloud Platform (GCP) requires a service account to access Vertex AI, simple API keys will not work. A token will be generated from the service account JSON file, which will then be used to authenticate requests to the Vertex AI API.

You can create a service account by following these steps:

**Prerequisites:**

1. You must have a Google Cloud Platform (GCP) account.
2. You must have a project created within your GCP account.
3. You must have billing enabled for that project.

#### Step 1: Enable the Vertex AI API

Before your key can work, the API must be enabled for your project.

1. Go to the Google Cloud Console: <https://console.cloud.google.com/>
2. Make sure the correct project is selected in the top bar.
3. Navigate to the Vertex AI API page: <https://console.cloud.google.com/apis/library/aiplatform.googleapis.com>
4. If it's not already enabled, click the "Enable" button.

#### Step 2: Create the Service Account

This is the identity that will be used to access the Vertex AI API.

1. In the Google Cloud Console, navigate to the "Service Accounts" page. You can search for it in the top search bar or use this direct link: <https://console.cloud.google.com/iam-admin/serviceaccounts>
2. Select your GCP project and click "+ CREATE SERVICE ACCOUNT".
3. Service account name: Give it a descriptive name, like `my-vertex-ai-client`.
4. Click "CREATE AND CONTINUE".
5. Grant this service account access to project: In the "Role" dropdown, search for and select Vertex AI User. This role grants the necessary permissions to run models without giving away too much access.
6. Click "CONTINUE", and then click "DONE".

#### Step 3: Generate the JSON Key

This is the "password" file you need. It contains sensitive information, so don't share it or upload it anywhere public.

1. You should now be back on the Service Accounts list. Find the account you just created (e.g., sillytavern-vertex-ai).
2. Click the three-dot menu (⋮) on the far right of that row and select "Manage keys".
3. Click "ADD KEY" -> "Create new key".
4. Ensure the Key type is set to JSON.
5. Click "CREATE".

A .json file will immediately be downloaded to your computer. Keep it safe, because this key can't be recovered if lost.

#### Step 4: Put the JSON Content into SillyTavern

The JSON file you downloaded contains all the information needed to authenticate with the Vertex AI API. It will look something like this:

```json
{
    "type": "service_account",
    "project_id": "your-gcp-project-name",
    "private_key_id": "...",
    "private_key": "-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----\n",
    "client_email": "sillytavern-vertex-ai@your-gcp-project-name.iam.gserviceaccount.com",
    "client_id": "...",
    "auth_uri": "https://accounts.google.com/o/oauth2/auth",
    "token_uri": "https://oauth2.googleapis.com/token",
    "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
    "client_x509_cert_url": "..."
}
```

1. Open the .json file you just downloaded with a simple text editor (like Notepad on Windows, TextEdit on Mac, or VS Code).
2. Select all the text in the file (Ctrl+A or Cmd+A).
3. Copy the text to your clipboard (Ctrl+C or Cmd+C).
4. In SillyTavern, go to the "API Connections" page, select "Chat Completion" as the API type, and then select "Google Vertex AI" from the dropdown menu. Switch the authentication method to "Service Account".
5. Paste the entire copied content into the "Service Account JSON Content" text box.
6. Click the "Validate JSON" button to make sure you copied it correctly.
7. Finally, scroll down and click "Connect" at the bottom of the API settings page.

You should now be able to use the Google Vertex AI API with SillyTavern.

### Express Mode

Express mode is the quickest way to get started with using Generative AI on Google Cloud. It allows you to use the Gemini API without needing to set up a service account. Instead, you can use an API key directly.

See the official documentation for more details: [Vertex AI in express mode overview](https://cloud.google.com/vertex-ai/generative-ai/docs/start/express-mode/overview).

#### Step 1: Ensure your account is eligible for Express Mode

You must have a Google account that was not previously used to create a Google Cloud project.
If you have an existing Google Cloud project (including free trials), you can create a new one for this purpose.

#### Step 2: Active the Vertex AI Express Mode

1. Go to the following web page: [Vertex AI Studio](https://cloud.google.com/generative-ai-studio).
2. Click on "Try it free".
3. Accept the terms and conditions and sign in with your Google account.
4. Choose your country and click "Agree & start free". Wait for the setup to complete.

#### Step 3: Create an API Key

1. Verify that your Google Cloud console is running in Express Mode. You should see a banner at the top left corner of the page.
2. Click on the "API Keys" link in the left sidebar.
3. Click on the "Create API Key" button.
4. A new API key will be generated. Copy this key to your clipboard.

#### Step 4: Put the API Key into SillyTavern

1. In SillyTavern, go to the "API Connections" page.
2. Select "Chat Completion" as the API type.
3. Select "Google Vertex AI" from the dropdown menu.
4. Switch the authentication method to "Express Mode (API Key)".
5. Paste the API key you copied earlier into the "API Key" text box.
6. Click the "Connect" button to save the key.

You should now be able to use the Google Vertex AI API in Express Mode with SillyTavern.
