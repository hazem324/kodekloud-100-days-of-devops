# Fork a Git Repository

There is a Git server utilized by the **Nautilus** project teams. A new developer named **Jon** has joined the team and needs to start working on an existing project. As part of the onboarding process, Jon must **fork** an existing Git repository using the Gitea web interface.

Follow the instructions below to complete the task.

---

## Task Objective

Fork the repository **`sarah/story-blog`** under the **jon** user account using the Gitea UI.

---

## Prerequisites

* Gitea server is accessible via the **Gitea UI button** in the top navigation bar
* Valid credentials:

  * **Username:** `jon`
  * **Password:** `Jon_pass123`

---

## Steps to Fork the Repository

1. Click on the **Gitea UI** button located on the top bar to open the Gitea web interface.
2. Log in using the following credentials:

   * Username: `jon`
   * Password: `Jon_pass123`
3. After logging in, use the search bar or repository list to locate the repository:

   * `sarah/story-blog`
4. Open the repository page.
5. Click the **Fork** button located at the **top-right corner** of the repository page.
6. When prompted, select **jon** as the target owner.
7. Wait for the forking process to complete.

 The repository should now appear under **jon/story-blog**.


---

## Good to Know

### Git Forking Concepts

* **Purpose:** Create a personal server-side copy of another user’s repository
* **Independence:** Changes in your fork do not affect the original repository
* **Contribution:** Common workflow in open-source and team-based development
* **Ownership:** Full control over your forked repository

### Fork vs Clone

| Fork                    | Clone                   |
| ----------------------- | ----------------------- |
| Server-side copy        | Local copy              |
| Created via UI          | Created via `git clone` |
| Used for collaboration  | Used for development    |
| Maintains upstream link | No automatic upstream   |

### Typical Collaboration Workflow

1. **Fork** the repository
2. **Clone** the fork locally
3. **Create a branch**
4. **Make changes**
5. **Commit and push**
6. **Open a Pull Request**

### Gitea Highlights

* Self-hosted Git service (GitHub-like)
* Lightweight and fast
* Supports forks, pull requests, issues, and wikis
* Ideal for private DevOps environments