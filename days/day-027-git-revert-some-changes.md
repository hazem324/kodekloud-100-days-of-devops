# Git Revert Some Changes

The Nautilus application development team was working on a git repository
/usr/src/kodekloudrepos/cluster present on Storage server in Stratos DC.
However, they reported an issue with the recent commits being pushed to this repo.
They have asked the DevOps team to revert repo HEAD to last commit.
Below are more details about the task:

- In /usr/src/kodekloudrepos/cluster git repository, revert the latest commit (HEAD)
  to the previous commit (JFYI the previous commit hash should be with initial commit message).
- Use revert cluster message (please use all small letters for commit message)
  for the new revert commit.

---

## Steps

1. Login into storage server

2. Move into repository directory

    ```sh
    sudo su
    cd /usr/src/kodekloudrepos/cluster
    ```

3. Check git log to find initial commit

    ```sh
    git log --oneline
    ```

    You should see the latest commit at the top and the previous commit
    with the message **initial commit**.

4. Ensure working tree is clean

    ```sh
    git status
    git clean -fd
    ```

    This step removes untracked files that may cause automated validation to fail.

5. Revert the latest commit (HEAD)

    ```sh
    git revert HEAD
    ```

    When the editor opens, replace the default message with:

    ```
    revert cluster
    ```

    Save and exit the editor.

6. Verify the revert

    ```sh
    git log --oneline
    ```

    The latest commit should now be:

    ```
    revert cluster
    ```

7. Push the changes (if required)

    ```sh
    git push
    ```

---

## 🧠 Good to Know

### Git Revert vs Reset

- **Revert**: Creates a new commit that undoes changes (safe)
- **Reset**: Moves HEAD pointer and can delete history (destructive)
- **Public History**: Always use revert for shared repositories
- **Private History**: Reset is acceptable only for local, unpublished changes

### Revert Operations

- **Single Commit**: `git revert <commit-hash>`
- **HEAD**: `git revert HEAD` — revert latest commit
- **Range**: `git revert HEAD~3..HEAD` — revert multiple commits
- **No Commit**: `git revert --no-commit` — stage revert without committing

### Commit Messages

- **Exact Match**: Must match task requirement exactly
- **Lowercase**: Follow instructions carefully (case-sensitive)
- **Clarity**: Indicates what was reverted
- **Traceability**: Preserves clear Git history

### Safety Considerations

- **Shared Repositories**: Never use `git reset --hard`
- **Validation Systems**: Extra or untracked files can cause task failure
- **Clean State**: Always verify `git status` before and after revert
- **Best Practice**: Preserve history for audit and rollback tracking
