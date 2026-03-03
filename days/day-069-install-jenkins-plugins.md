# Install Jenkins Plugins

The Nautilus DevOps team has recently set up a Jenkins server to be used for CI/CD jobs. Before creating pipelines, required plugins must be installed to enable source control and repository integration.

This task covers installing the **Git** and **GitLab** plugins from the Jenkins Update Center.

---

## Prerequisites

* Jenkins server is up and running
* Access to Jenkins Web UI
* Admin credentials:

  * **Username:** `admin`
  * **Password:** `Adm!n321`

---

## Steps

### 1. Login to Jenkins

1. Click on the **Jenkins** button from the top bar.
2. Login using:

   ```
   Username: admin
   Password: Adm!n321
   ```


### 2. Access Plugin Manager

1. From Jenkins dashboard → Click **Manage Jenkins**
2. Click **Manage Plugins**
3. Open the **Available** tab


### 3. Install Git Plugin

1. Search for `Git`
2. Select:

   *  Git Plugin
3. Click **Install without restart**

   * OR choose **Download now and install after restart**


### 4. Install GitLab Plugin

1. In the Available tab, search for `GitLab`
2. Select:

   *  GitLab Plugin
3. Click **Install**


### 5. Restart Jenkins (If Required)

If prompted:

 Select **Restart Jenkins when installation is complete and no jobs are running**

Or restart manually:

```bash
sudo systemctl restart jenkins
```

⚠ After restart, wait until the Jenkins login page reappears before proceeding.


## Verify

To verify successful installation:

1. Go to **Manage Jenkins**
2. Click **Manage Plugins**
3. Open the **Installed** tab
4. Search for:

   * Git Plugin
   * GitLab Plugin

Both plugins should appear in the Installed list.

---

## Issues & Solutions

| Issue                             | Solution                                                     |
| --------------------------------- | ------------------------------------------------------------ |
| Plugin installation fails         | Go to **Updates** tab and update required dependencies first |
| Timeout or download error         | Retry installation                                           |
| Plugins not visible after install | Restart Jenkins                                              |
| Dependency conflict               | Update all existing plugins before installing new ones       |
| Jenkins stuck during restart      | Restart service manually using systemctl                     |

---

## Good to Know

### Jenkins Plugin Management

* **Plugin Ecosystem**: Jenkins has thousands of plugins extending its core functionality.
* **Update Center**: Centralized repository where plugins are downloaded and updated.
* **Dependencies**: Some plugins require other plugins to function properly.
* **Restart Requirement**: Certain plugins require a Jenkins restart to activate.


### Essential Plugins for CI/CD

| Plugin   | Purpose                                               |
| -------- | ----------------------------------------------------- |
| Git      | Enables source code cloning from Git repositories     |
| GitLab   | Enables GitLab integration and webhook triggers       |
| Pipeline | Allows defining CI/CD pipelines as code (Jenkinsfile) |
| SSH      | Enables remote server communication                   |


### Plugin Installation Best Practices

* Update existing plugins before installing new ones
* Avoid restarting while jobs are running
* Install only necessary plugins (avoid plugin overload)
* Keep plugins updated for security and compatibility


### Troubleshooting Tips

* Ensure Jenkins server has internet connectivity
* Check `/var/log/jenkins/jenkins.log` for errors
* Restart Jenkins in safe mode if needed
* Retry installation if download fails


## Documentation Requirement

For tasks involving UI configuration:

* Capture screenshots of:

  * Dashboard login
  * Plugin installation page
  * Installed plugins list
  * Restart confirmation page
* Or use screen recording tools (e.g., Loom)

