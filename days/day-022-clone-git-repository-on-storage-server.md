# Clone Git Repository on Storage Server

The DevOps team created a new Git repository that is currently unused. The Nautilus application development team needs a local copy of this repository on the Storage Server in the Stratos DC. This task involves cloning the repository without making any changes to its content or configuration.

---

## Repository Details

* **Source Repository:** `/opt/blog.git`
* **Destination Directory:** `/usr/src/kodekloudrepos`
* **Operation:** Clone only 

---

## Steps

1. **Log in to the Storage Server**

   ```sh
   ssh natasha@<storage-server-ip>
   ```

2. **Navigate to the destination directory**

   ```sh
   cd /usr/src/kodekloudrepos
   ```

3. **Clone the Git repository**

   ```sh
   git clone /opt/blog.git
   ```

4. **Verify the clone**

   ```sh
   ls
   cd blog
   git status
   ```

Expected output:

```text
On branch main
nothing to commit, working tree clean
```

---

## 🧠 Good to Know

### Git Clone Operations

* **Purpose**: Creates a local copy of a remote or local Git repository
* **Full History**: Includes all commits, branches, and tags
* **Remote Tracking**: Automatically sets the `origin` remote
* **Working Directory**: Checks out the latest commit into a working tree

### Clone Sources

* **Local Path**

  ```sh
  git clone /opt/blog.git
  ```
* **HTTP / HTTPS**

  ```sh
  git clone https://github.com/user/repo.git
  ```
* **SSH**

  ```sh
  git clone git@github.com:user/repo.git
  ```
* **Git Protocol**

  ```sh
  git clone git://server/repo.git
  ```

### Common Clone Options

* **Shallow Clone**

  ```sh
  git clone --depth 1 /opt/blog.git
  ```
* **Clone Specific Branch**

  ```sh
  git clone --branch dev /opt/blog.git
  ```
* **Bare Clone**

  ```sh
  git clone --bare /opt/blog.git
  ```
* **Mirror Clone**

  ```sh
  git clone --mirror /opt/blog.git
  ```

### Post-Clone Behavior

* **Remote `origin`** is automatically configured
* **Default branch** is checked out
* **Tracking branches** are created for push/pull
* **Repository configuration** is preserved
