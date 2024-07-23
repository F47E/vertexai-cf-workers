# vertexai-cf-workers

## Prerequisites
1. **Sign up for a GCP account:**
   - Go to [https://cloud.google.com/vertex-ai](https://cloud.google.com/vertex-ai) and sign up for a GCP account.
   - You can get $150 free credits without a credit card, or $300 free credits by providing a credit card. (Note that the free credits expire in 90 days)

2. **Enable Vertex AI API:**
   - Go to [https://console.cloud.google.com/marketplace/product/google/aiplatform.googleapis.com](https://console.cloud.google.com/marketplace/product/google/aiplatform.googleapis.com) to enable the Vertex AI API for your project.

3. **Apply for Claude models:**
   - Go to [https://console.cloud.google.com/vertex-ai](https://console.cloud.google.com/vertex-ai) and apply for access to the Claude models.

4. **Create a [Service Account](https://console.cloud.google.com/projectselector/iam-admin/serviceaccounts/create?walkthrough_id=iam--create-service-account#step_index=1):**
   - Select the project ID you created earlier.
   - Make sure to grant the role of "Vertex AI User" or "Vertex AI Administrator" to the service account.
   - On the service account page you just created, go to the "Keys" tab and click "Add Key".
   - Select "Create new key" and choose "JSON" as the key type.
   - The key file will be downloaded automatically. This file contains the required variables for the worker, such as `project_id`, `private_key`, and `client_email`.

5. **Set up Cloudflare Workers:**
   - Follow Cloudflare's [Workers documentation](https://developers.cloudflare.com/workers/) to deploy and configure your worker script.

## Workers Variables

The worker requires several environment variables to be set:

- `CLIENT_EMAIL`: This is the email associated with your GCP service account. You can find this in your service account's JSON key file.
- `PRIVATE_KEY`: This is the private key associated with your GCP service account. You can find this in your service account's JSON key file.
- `PROJECT`: This is the ID of your GCP project. You can find this in your service account's JSON key file.
- `API_KEY`: This is a string that you define. It is used to authenticate requests to the worker.

## Updating the HTML File

After setting up Cloudflare and GCP, you need to update the HTML file with the appropriate information:

1. **Update the API URL and Key in the JavaScript Code:**
   - In the `<script>` section of your HTML file, replace the placeholder for the API endpoint URL with the URL of your deployed Cloudflare Worker. For example:

     ```javascript
     const response = await fetch('https://your-worker-subdomain.workers.dev/v1/messages', {
         method: 'POST',
         headers: {
             'Content-Type': 'application/json',
             'x-api-key': 'your-api-key-here',
             'anthropic-version': '2023-06-01'
         },
         body: JSON.stringify({
             model: "claude-3-5-sonnet-20240620",
             messages: currentChat,
             max_tokens: 4096
         })
     });
     ```

   - Replace `'https://your-worker-subdomain.workers.dev/v1/messages'` with your actual Cloudflare Worker URL.

   - Replace `'your-api-key-here'` with the API key you defined for authentication.

2. **Ensure the API Key and Endpoint Match:**
   - Make sure that the API key and endpoint in your HTML code match those configured in your Cloudflare Worker and GCP settings.
  
3. **Run the HTML file**
   - Open the "claude-chat-interface-cloudflare.html" that you just edited from this repo and it should function if you did it right.

By following these steps, you’ll ensure that the HTML file correctly interfaces with your Cloudflare Worker and GCP setup.
