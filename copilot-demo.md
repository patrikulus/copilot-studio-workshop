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
- Create a new GitHub issue using a Power Automate flow.
The agent will operate in a professional tone and be used by development teams inside Microsoft Teams. It should include example phrases like "Show my PRs", "List open issues", and "Create a new issue".
```
4. Click **Create**
5. Explore and modify (if you want):
   - **Greeting Topic**: e.g., “Hi! I can help you with GitHub issues and PRs.”
   - **Fallback Topic**: Default or add custom handling.

---

## 🔗 2. Connect to GitHub (via Custom Action)

### Create a GitHub Fine-Grained Personal Access Token (PAT)

1. Visit [https://github.com/settings/personal-access-tokens](https://github.com/settings/personal-access-tokens)
2. Click **Generate new token (fine-grained)**
3. Configure the following:

   * **Repository access**: Select **Only select repositories** and choose `copilot-studio-workshop`
   * **Permissions**: Grant `Contents: Read and write` and `Pull requests: Read and write`
4. Generate and copy the token. You will not see it again.

### Use Custom Action with GitHub REST API
1. Go to **Topics** > **Add a topic**: 
   - Name your topic: `"Get pull requests"`.
   - Create a topic to...: `"Let user list latest GitHub pull requests"`.
2. Click **Create**
3. Add a **Call an action** node > Choose **+ Add a new action**.
4. Select **HTTP request**:
   - **Method**: `GET`
   - **URL**: `https://api.github.com/repos/patrikulus/<repo>/pulls?state=open`
   - **Headers**:
     - User-Agent: "Mozilla/5.0",
     - Accept: "application/vnd.github+json",
     - Authorization: "Bearer <PAT>"

5. Set output variable for the response.

---

## ⚙️ 3. Create Power Automate Flow: Create GitHub Issue

### ⚙️ A. Get Pull Requests from GitHub

* Trigger: "When an action is called from a Copilot"
* Inputs: `owner`, `repo`
* Action: Use **GitHub connector**:

  * Action: **List pull requests**
  * Filter by state (e.g., open)
* Output: Return a formatted summary string of recent PRs

### ⚙️ B. Create GitHub Issue

* Trigger: "When an action is called from a Copilot"
* Inputs: `title`, `body`
* Action: Use **GitHub connector**:

  * Action: **Create issue** in a specific repo
  * Authenticate using your fine-grained token
* Output: confirmation string (e.g., "Issue created: #42")

---

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



