
# Jenkins-GitHub-Telegram Integration

Hi! I’m documenting how I set up my Jenkins pipeline to integrate with GitHub and send build status notifications to my Telegram account. This is my step-by-step process, including how I used `config.xml` to configure the job initially, connected it to my GitHub repo, and added Telegram notifications. Feel free to follow along if you’re setting up something similar!

---

## Prerequisites
- I’m running Jenkins on a CentOS VM.
- I’ve installed ngrok to expose Jenkins at `https://de72-102-89-84-203.ngrok-free.app` (your URL will differ).
- My GitHub repo is `https://github.com/div-ops123/Jenkins-GitHub-Webhook-Integration.git`.
- I have a Telegram account on my phone.
- The GitHub webhook is already configured to trigger builds on push.

---

## Step 1: Initial Job Setup with `config.xml`
I started by setting up my Jenkins job manually using a `config.xml` file because I wanted precise control over the initial configuration. Here’s how I did it:

1. I wrote a basic `config.xml` file. 

2. To Create the Job, in my VM:
First, get CSRF crumb:

```
bash
curl -u <username>:<token> <jenkins url:port number>/crumbIssuer/api/json
```
**note the crumb**

Second, create the job
```
bash
curl -X POST -u <your_username>:<your_token> -H "Jenkins-Crumb:<crumb>" -H "Content-Type:application/xml" --data-binary @config.xml -o /dev/null -w "%{http_code}\n" http://localhost:<your_VM_port>/createItem?name=<job_name>
```

3. Verify Job Creation:
 - In the terminal, you should see 200 status code.
 - In the Jenkins UI, confirm the job was created.
 - Push a commit to your repo main branch, confirm job build in Jenkins UI.   

Using `config.xml` to create the Jenkins job helps for automation and replication across evironments, without clicking through the UI.

---

## Step 3: Setting Up Telegram Notifications
I wanted build updates on my phone, so I integrated Jenkins with Telegram. Here’s how I did it:

### 3.1 Create a Telegram Bot
- I opened Telegram and searched for `@BotFather`.
- I sent `/newbot`, named it, and set the username.
- I copied the **API token**.

### 3.2 Get My Chat ID
- I searched for `@getmyid_bot` in Telegram.
- Sent `/start`, then `/getid`, and copied my **chat ID**.

### 3.3 Install the Telegram Plugin
- In Jenkins, I went to “Manage Jenkins” > “Manage Plugins” > “Available.”
- Searched for “Telegram,” installed the **Telegram Notifications Plugin**, and restarted Jenkins.

### 3.4 Set Credentials
- I went to “Manage Jenkins” > “Credentials” and scrolled to “Stores scoped to Jenkins” > "System" > "Global credentials (unrestricted)" then "Add Credentials"
- Entered:
  - **Kind**: `Secret Text
  - **Secret**: Paste your Telegram Bot Token
- Create.
- Create another new credential for "Telegram Chat ID"

### 3.5 Update My Jenkinsfile
I edited my `Jenkinsfile` in the repo to send Telegram notifications.
- I committed and pushed it to my repo, triggering the webhook.

---

## Step 4: Testing It Out
- I pushed a small change to my repo to test the pipeline.
- On Telegram, I got the message!
- It worked perfectly, and I could monitor builds from my phone!

---

## Notes
- **ngrok**: I kept ngrok running (`ngrok http 8081`) to expose Jenkins on port 8081 to a public url for the GitHub webhook.
- **Credentials**: I stored them securely in Jenkins credentials instead of hardcoding.
---

## Troubleshooting
- **No Telegram Messages**: I’d check the bot token and chat ID in Jenkins, and look at the Jenkins job console output in the Jenkins UI.
- **failed to connect to host**: If the webhook says "failed to connect to host". I'd verify my VM where I am running the Jenkins is running, I am exposing the Jenkins port to a public ip by using *ngrok*.

---

That’s it! I now have a Jenkins pipeline that builds my code on every push and notifies me on Telegram. The Telegram integration keeps me updated on the go. If you’ve got questions about my setup, let me know—I’m happy to help!
