# 🤖 Copilot Studio Demo: Custom Agent + GitHub + Power Automate

In this step-by-step guide, you’ll learn how to create a custom Copilot agent that:
- Retrieves data from a GitHub repository (e.g., pull requests, issues)
- Creates a GitHub issue using Power Automate

---

## 🧱 1. Create a Copilot Agent

1. Navigate to [Copilot Studio](https://copilotstudio.microsoft.com/).
2. Click **Create** on the left menu bar, then **+ New Agent**
3. Enter a description for AI to create the agent. e.g:
```
Create a Copilot agent called "DevOps Helper Bot" that assists developers with GitHub repository tasks. It should be able to:
- List recent pull requests and issues from a specific repository.
- Filter pull requests where the user is a reviewer.
- Create a new GitHub issue using a Power Automate flow.
The agent will operate in a professional tone and be used by development teams inside Microsoft Teams. It should include example phrases like "Show my PRs", "List open issues", and "Create a new issue".
```
4. 2. Click **Create**
5. Set up:
   - **Greeting Topic**: e.g., “Hi! I can help you with GitHub issues and PRs.”
   - **Fallback Topic**: Default or add custom handling.

---

## 🔗 2. Connect to GitHub (via Custom Action)

### Option A: Use GitHub Plugin (if available)
1. Go to **Plugins** tab and search for **GitHub**.
2. If enabled, configure it and use actions like `GetIssues`, `ListPRs`.

### Option B: Use Custom Action with GitHub REST API
1. Go to **Topics** > Create topic: `"Show my pull requests"`.
2. Add a **Call an action** node > Choose **+ Add a new action**.
3. Select **HTTP request**:
   - **Method**: `GET`
   - **URL**: `https://api.github.com/repos/<owner>/<repo>/pulls?state=open`
   - **Authentication**: Personal Access Token (scoped for `repo`)
4. Parse and return pull request titles in response.

---

## ⚙️ 3. Create Power Automate Flow: Create GitHub Issue

1. Open [Power Automate](https://make.powerautomate.com/).
2. Click **Create** > **Instant cloud flow** > Name: `CreateGitHubIssue`.
3. Trigger: **When an action is called from a Copilot**.
4. Add inputs:
   - `title` (Text)
   - `body` (Text)
5. Add **HTTP** or **GitHub Connector** action:
   - POST to `https://api.github.com/repos/<owner>/<repo>/issues`
   - Headers: `Authorization: Bearer <PAT>`
   - Body:
     ```json
     {
       "title": "@{triggerBody()['title']}",
       "body": "@{triggerBody()['body']}"
     }
     ```
6. Return confirmation message as output.

---

## 🧩 4. Add Flow to Copilot Agent

1. In Copilot Studio, create a topic: `"Create a GitHub issue"`.
2. Add trigger phrases: “open issue”, “report bug”, etc.
3. Insert **Call an action** node.
4. Select your `CreateGitHubIssue` flow.
5. Map input fields from conversation.
6. Add a final message node:
   - “✅ Your GitHub issue has been created!”

---

## 🧪 5. Test and Publish

1. Use the **Test chat** panel:
   - Try “Show my pull requests”
   - Try “Create a new issue”
2. Adjust logic or inputs as needed.
3. Click **Publish**, then **Share in Teams** or copy embed link.

---

🎉 Congrats! You’ve created a custom DevOps assistant powered by Copilot Studio.

Need help? Open an issue in this repo or ask during the session!
