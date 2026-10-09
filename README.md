# Team Workflow — Arena AI Workspace + GitHub

How we develop in this repo: every team member works in **their own Arena AI conversation**
(an isolated sandbox), pushes to **this GitHub repository** — our single source of truth —
and opens **pull requests** to merge. **GitHub Desktop** is optional and used only to
visualize history and diffs.

| Tool | Role | Auth |
|---|---|---|
| Arena AI workspace | Edit scripts with the agent, commit, push | Your own fine-grained PAT |
| GitHub repository | Single source of truth · protected `main` | — |
| GitHub Desktop (optional) | Visualize history & diffs on your PC | Normal sign-in, no token |

---

## 1 · One-time setup (on github.com)

1. **Get repo access** — ask the repo owner to add your GitHub account.
2. **Create your OWN fine-grained Personal Access Token:**
   GitHub → *Settings → Developer settings → Personal access tokens → Fine-grained tokens → Generate new token*
   - Repository access: **this repo only**
   - Permissions → Contents: **Read and write** ⚠️ *critical — see below*
   - Expiry: short (e.g. 30 days)

   > [!IMPORTANT]
   > **Contents must be "Read and write", not "Read-only".** A read-only token still lets the
   > agent clone and pull (reads), so setup *looks* fine — but every push fails with
   > `Permission to <repo> denied to <user>` / `Resource not accessible by personal access token`.
   > This is the most common setup mistake; the fix is under *Troubleshooting* (no new token needed).

   > [!WARNING]
   > Never share your token — not in chat, not in another member's Arena session.
   > Whoever holds a token acts as *you*.
3. **(Optional)** Install [GitHub Desktop](https://desktop.github.com/) and sign in —
   it only pulls/visualizes, so no token is needed there.

## 2 · Start an Arena conversation (every new session)

Open **your own** Arena session and send this as your **first message**
(fill in your token, name, email and branch):

```text
Clone https://github.com/kamkammal/test-one.git into the workspace and pull the latest main.
Here is my fine-grained PAT (this repo only, Contents: read & write): <PASTE_TOKEN_HERE>
Configure git (user.name "YOUR NAME", user.email "YOUR@EMAIL"), re-add the remote with the token,
then check out my branch feature/<name>-<topic> — create it from main if it doesn't exist yet.
```

> [!NOTE]
> The sandbox doesn't keep `.git/config` between sessions, so the remote and token must be
> set up again each time — the prompt above handles that automatically.

## 3 · Work loop

Instruct the agent task by task; it edits the scripts and commits inside the sandbox.
Use one prompt per change:

```text
In scripts/<file>.py, update <what> so that <goal>. Don't touch unrelated code — when done,
commit the change with a clear message and show me the diff.
```

## 4 · Push & open a pull request

```text
Commit anything outstanding and push my branch feature/<name>-<topic> to GitHub.
```

Then on github.com: **Compare & pull request** → describe the change → **Create pull request**.
The team lead reviews and merges once clean. `main` is protected — PRs only.

## 5 · Stay in sync

- In your **next** Arena session, the session-start prompt (step 2) automatically pulls the latest `main`.
- Mid-session, after someone's PR merges, ask the agent:

```text
Pull the latest main into my workspace and summarize what changed since my branch.
```

- GitHub Desktop users: click **Fetch / Pull** on your PC instead — no prompt or token needed.

---

## Branching convention

- Branch name: `feature/<name>-<topic>` (e.g. `feature/aina-export-csv`)
- `main` is protected: no direct pushes, PRs required
- Pull before starting work; push early and often so conflicts stay small

## Golden rules

1. **GitHub is the single source of truth.** Arena sandboxes are isolated — nothing syncs between sessions except through this repo.
2. **Pull before starting work; push early and often.**
3. **One token per person, never shared** — commits stay attributed to you.

## Troubleshooting

| Problem | Fix |
|---|---|
| Token expired | Regenerate it on github.com with the same scopes and paste the new one into your session-start prompt |
| `Repository not found` / auth errors | Re-send the session-start prompt — it re-adds the remote with your token |
| Push fails: `Permission to … denied to <you>` or `Resource not accessible by personal access token` | Your token is read-only. Edit it on github.com: Fine-grained tokens → your token → **Edit** → Contents: **Read and write**. Editing keeps the same token — then just re-send the push prompt |
| Merge conflict | Ask the agent: *"Pull main into my branch and help me resolve the conflicts"* |

---

*Companion visual diagram: [`arena-team-github-workflow.html`](./docs/arena-team-github-workflow.html) (open in any browser).*
