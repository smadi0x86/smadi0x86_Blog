---
description: >-
  Git is a distributed version control system that allows multiple developers to
  work on the same codebase without interfering with one another.
cover: https://images3.alphacoders.com/109/1091500.jpg
coverY: 0
---

# Git

## <mark style="color:red;">**Setup & Configuration**</mark>

### <mark style="color:yellow;">**Installing Git**</mark>

#### <mark style="color:purple;">Git is a distributed version control system, to install it (linux-based systems):</mark>

```bash
sudo apt-get install git
```

### <mark style="color:yellow;">**Configuring Git**</mark>

#### <mark style="color:purple;">**Set username and email:**</mark> This sets your identification for every commit.

```bash
git config --global user.name "Your Name"
git config --global user.email "youremail@example.com"
```

#### <mark style="color:purple;">**List all global configurations:**</mark> Shows your git configuration details.

```bash
git config --global --list
```

#### <mark style="color:purple;">**Unset a specific configuration:**</mark> For instance, to remove the globally set username.

```bash
git config --global --unset user.name
```

### <mark style="color:yellow;">**Setting Up Authentication for GitLab**</mark>

#### <mark style="color:purple;">**SSH Authentication:**</mark>

```bash
ssh-keygen -t rsa -b 4096 -C "youremail@example.com"
```

```bash
cat ~/.ssh/id_rsa.pub
```

## <mark style="color:red;">**Repository Operations**</mark>

### <mark style="color:yellow;">**Setting Up GitLab Repository**</mark>

#### <mark style="color:purple;">A step-by-step guide to set up a new project on GitLab:</mark>

1. Login to GitLab.
2. Click on the "+" icon at the top right.
3. Select "New project".

### <mark style="color:yellow;">**Cloning a Repository**</mark>

#### <mark style="color:purple;">Copy a remote repository to your local machine:</mark>

```bash
git clone https://gitlab.com/yourusername/yourrepository.git
```

### <mark style="color:yellow;">**Integrating an Existing Folder with GitLab**</mark>

#### <mark style="color:purple;">Link your local folder with a GitLab repository:</mark>

```bash
cd /path/to/your/folder
git init
git remote add origin git@gitlab.com:yourusername/yourrepository.git
git add .
git commit -m "Initial commit or relevant message"
git push -u origin main
```

## <mark style="color:red;">**Regular Workflow & Common Commands**</mark>

### <mark style="color:yellow;">**Synchronize with Remote Repository**</mark>

#### <mark style="color:purple;">Update your local branch with the latest changes from the remote repository:</mark>

```bash
git pull
```

### <mark style="color:yellow;">**Committing Changes**</mark>

#### <mark style="color:purple;">After making your desired changes, save them to your local repository and then push to the remote repository:</mark>

```bash
git add .
git commit -m "Descriptive commit message"
git push
```

## <mark style="color:red;">**Branch Management**</mark>

### <mark style="color:yellow;">**Creating, Renaming, and Deleting Branches**</mark>

#### <mark style="color:purple;">Handle multiple lines of development within a single repository:</mark>

```bash
git branch new_branch_name
git branch -m old_name new_name
git branch -d branch_name
git push origin --delete branch_name
```

### <mark style="color:yellow;">**Switching Between Branches & Commits**</mark>

#### <mark style="color:purple;">Move between different versions or branches within the repository:</mark>

```bash
git checkout branch_name
git checkout commit_hash
```

### <mark style="color:yellow;">Restoring Commited Branches</mark>

If you've accidentally removed a branch in Git, don't worry! You can typically recover it with a few simple steps.

Every action in git is logged.&#x20;

#### <mark style="color:purple;">To find the last commit of the deleted branch:</mark>

```bash
git reflog
```

#### <mark style="color:purple;">For example, you might see an entry like:</mark>

```less
abcdef1 HEAD@{2}: checkout: moving from feature-branch to main
```

{% hint style="info" %}
As an example `abcdef1` is the commit hash.
{% endhint %}

Now that you have the commit hash, you can restore the branch.&#x20;

#### <mark style="color:purple;">Let's assume you want to recover a branch named</mark> <mark style="color:purple;"></mark><mark style="color:purple;">`feature-branch`</mark><mark style="color:purple;">:</mark>

```bash
git checkout -b feature-branch abcdef1
```

## <mark style="color:red;">**Conflict Resolution & Reversion**</mark>

### <mark style="color:yellow;">**Handling Merge Conflicts**</mark>

#### <mark style="color:purple;">When merging or pulling, conflicts may occur. These steps guide you through resolution:</mark>

```bash
git add resolved_file.ext
git commit
```

### <mark style="color:yellow;">**Reverting and Resetting Changes**</mark>

#### <mark style="color:purple;">Undo changes in your repository:</mark>

```bash
git revert commit_hash
git reset --hard branch_or_commit_name
```

## <mark style="color:red;">**Advanced Operations**</mark>

### <mark style="color:yellow;">**Stashing, Tagging, and Searching**</mark>

#### <mark style="color:purple;">Various advanced operations for better Git management:</mark>

```bash
git stash
git stash apply
git stash drop
git tag v1.0 commit_hash
git log --grep="search_term"
git grep "search_term"
```

### <mark style="color:yellow;">**Viewing Commit History & Using git blame**</mark>

#### <mark style="color:purple;">Examine the modification history of your repository:</mark>

```bash
git log
git log --oneline --graph --all
git blame filename.ext
```

## <mark style="color:red;">**Troubleshooting & Miscellaneous**</mark>

### <mark style="color:yellow;">**Dealing with Authentication Issues**</mark>

#### <mark style="color:purple;">Switch between SSH and HTTPS, or troubleshoot common authentication issues:</mark>

```bash
git remote set-url origin git@gitlab.com:yourusername/yourrepository.git
```

### <mark style="color:yellow;">**Update Default Branch on GitLab**</mark>

#### <mark style="color:purple;">Change the primary branch of your GitLab repository:</mark>

1. Go to your project.
2. Navigate to "Settings" > "Repository".
3. Set the new default branch.

## <mark style="color:red;">**Detailed Commit Inspection**</mark>

### <mark style="color:yellow;">**Show Commit Details**</mark>

#### <mark style="color:purple;">Display changes associated with a specific commit:</mark>

```bash
git show commit_hash
```

### <mark style="color:yellow;">**Reflog**</mark>

#### <mark style="color:purple;">Shows a log of where your HEAD and branch references have been:</mark>

```bash
git reflog
```

## <mark style="color:red;">**Working with Remotes**</mark>

### <mark style="color:yellow;">**List Remote Repositories**</mark>

#### <mark style="color:purple;">Show all remote repositories connected to the local repository:</mark>

```bash
git remote -v
```

### <mark style="color:yellow;">**Add Remote Repository**</mark>

#### <mark style="color:purple;">Connect a new remote repository to the local repository:</mark>

```bash
git remote add remote_name remote_url
```

### <mark style="color:yellow;">**Remove Remote Repository**</mark>

#### <mark style="color:purple;">Remove a connection to a remote repository:</mark>

```bash
git remote remove remote_name
```

### <mark style="color:yellow;">**Fetch from Remote**</mark>

#### <mark style="color:purple;">Download branches and/or tags from another repository, without making any changes to your local branches:</mark>

```bash
git fetch remote_name
```

## <mark style="color:red;">**Patching**</mark>

### <mark style="color:yellow;">**Creating a Patch**</mark>

#### <mark style="color:purple;">Create a patch file of the changes between two commits:</mark>

```bash
git format-patch older_commit_hash^..newer_commit_hash
```

### <mark style="color:yellow;">**Apply a Patch**</mark>

#### <mark style="color:purple;">Apply changes from a patch file to the code:</mark>

```bash
git apply patch_name.patch
```

## <mark style="color:red;">**Rebasing**</mark>

### <mark style="color:yellow;">**Rebase**</mark>

#### <mark style="color:purple;">Apply a series of commits from one branch onto another, effectively re-writing history. Useful for a clean commit history but can be dangerous if not understood well:</mark>

```bash
git rebase branch_name
```

### <mark style="color:yellow;">**Abort Rebase**</mark>

#### <mark style="color:purple;">Stop the rebase process and return to the initial state:</mark>

```bash
git rebase --abort
```

### <mark style="color:yellow;">**Continue Rebase After Conflict Resolution**</mark>

#### <mark style="color:purple;">Continue the rebase process after resolving merge conflicts:</mark>

```bash
git rebase --continue
```

### <mark style="color:yellow;">**Cleaning Up**</mark>

#### <mark style="color:purple;">Remove untracked files from the working directory. This doesn't remove untracked folders or files specified in</mark> <mark style="color:purple;"></mark><mark style="color:purple;">`.gitignore:`</mark>

```bash
git clean -n    # Shows which files would be removed
git clean -f    # Force remove files
```

### <mark style="color:yellow;">**Prune**</mark>

#### <mark style="color:purple;">Remove objects that are no longer pointed to by any commit. This cleans up your local repository:</mark>

```bash
git gc    # Garbage collector
git prune
```

## <mark style="color:red;">**Submodules**</mark>

### <mark style="color:yellow;">**Add a Submodule**</mark>

#### &#x20;<mark style="color:purple;">Add another repository as a submodule in the current repository. Useful when using other projects or shared code:</mark>

```bash
git submodule add repository_url path_to_add_submodule
```

### <mark style="color:yellow;">**Update Submodule**</mark>

#### <mark style="color:purple;">Update the registered submodules:</mark>

```bash
git submodule update --init --recursive
```

### <mark style="color:yellow;">**Bisect**</mark>

#### <mark style="color:purple;">Find by binary search the commit that introduced a bug:</mark>

```bash
git bisect start
git bisect bad                 # Specify the commit where the bug is present
git bisect good commit_hash    # Specify the latest commit where the bug is absent
```

Then, Git will check out a commit in the middle of that range.&#x20;

Test the code, and then mark that commit as `good` or `bad`.&#x20;

Repeat the process until the culprit commit is found. Finally:

```bash
git bisect reset   # End the bisect session
```
