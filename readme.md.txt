Asana <-> USPTO Integration

This server listens for updates in your Asana project. When a task's "Application Number" custom field is filled out, it automatically pulls data from the USPTO Open Data API and populates the task with the Patent Title and Status.

Step 1: Push to GitHub

Create a new repository on GitHub.

Add server.js, package.json, and setup-webhook.js to the repository.

Commit and push the files.

Step 2: Deploy the Server

Since Asana Webhooks require a public URL to send data to, you must host this code.

Go to a free hosting service like Render.com or Railway.app.

Create a new Web Service and link it to your GitHub repository.

The platform will automatically install dependencies (npm install) and start your app using npm start.

Copy your live URL (e.g., https://your-app-name.onrender.com). Add /webhook to the end of it for the next steps.

Step 3: Configure Environment Variables

In your hosting provider's dashboard (Render/Railway), add the following Environment Variables (Secrets):

ASANA_PAT: Your Asana Personal Access Token (Create this in Asana -> My Settings -> Apps -> Developer Apps -> Personal Access Tokens).

USPTO_API_KEY: Your USPTO Open Data Portal API Key.

CF_APP_NUMBER_GID: The Asana Global ID for your "Application Number" custom field.

CF_PATENT_TITLE_GID: The Asana Global ID for your "Patent Title" custom field.

CF_STATUS_GID: The Asana Global ID for your "Status" custom field.

(To find custom field GIDs, you can click on a task in Asana in the browser and look at the API payload using https://app.asana.com/api/1.0/tasks/TASK_ID, or use the Asana API explorer).

Step 4: Register the Webhook

Once your server is live, you need to tell Asana to start sending data to it.

Locally on your computer:

Run npm install

Create a .env file and add:

ASANA_PAT=your_asana_token
ASANA_PROJECT_GID=your_project_id_from_the_url
SERVER_URL=[https://your-app-name.onrender.com/webhook](https://your-app-name.onrender.com/webhook)


Run npm run setup-webhook.

If successful, Asana will do a "handshake" with your live server, and start monitoring the project!

Now, anytime you type an application number into Asana, your server will detect it and update the task automatically.