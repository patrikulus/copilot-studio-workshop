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

## ⚙️ 3. Create Power Automate Flows

Click **Flows** on the left sidebar, then **+ New agent flow** for each use case. After creating each flow you need to Save a draft, then rename it in Overview tab (they got "Untitled" name by default) and publish it.

### ⚙️ A. Get Pull Requests from GitHub

* Trigger: "When an action is called from a Copilot"
* Action: **Get all Pull Requests of A Repository**
   * Repository Owner: **patrikulus**
   * Repository Name: **copilot-studio-workshop**
* Output: **Respond to the agent**
   * Parameters:
      - name: result
      - value: **Body** variable (use / > Insert dynamic content)

### ⚙️ B. Create GitHub Issue

* Trigger: "When an action is called from a Copilot"
* Action: **Create an issue**
   * Repository Owner: **patrikulus**
   * Repository Name: **copilot-studio-workshop**
* Output: **Respond to the agent**
   * Parameters:
      - name: result
      - value: "Issue created" (or any other confirmation text)

---

## 🧩 4. Add Flows to Copilot Agent

1. In Copilot Studio, go to **Agents** > **DevOps Helper Bot** 
2. Switch to **Actions** tab and **+ Add an action**
3. Your flows should be listed, select then (one by one)
4. Disable the previously created Topic to avoid issue with getting pull requests, (both Topic and Action share the same name)

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



