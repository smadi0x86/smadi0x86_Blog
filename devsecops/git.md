---
description: >-
  Git is a distributed version control system that allows multiple developers to
  work on the same codebase without interfering with one another.
---

# Git

## <mark style="color:red;">Installing Git</mark>

#### <mark style="color:purple;">If not already installed:</mark>

```bash
sudo apt-get install git
```

## <mark style="color:red;">Configuring Git</mark>

#### <mark style="color:purple;">Set your username and email:</mark>

```bash
git config --global user.name "Your Name"
git config --global user.email "youremail@example.com"
```

#### <mark style="color:purple;">To list all global configurations:</mark>

```bash
git config --global --list
```

#### <mark style="color:purple;">To unset a specific configuration (e.g., username):</mark>

```bash
git config --global --unset user.name
```

## <mark style="color:red;">Setting Up GitLab</mark>

### <mark style="color:yellow;">Create a New Repository</mark>

* Login to GitLab.
* Click on the "+" icon at the top right and select "New project".

### <mark style="color:yellow;">Cloning a GitLab Repository</mark>

```bash
git clone https://gitlab.com/yourusername/yourrepository.git
```

## <mark style="color:red;">**Regular Git Workflow**</mark>

### <mark style="color:yellow;">**Pull Latest Changes**</mark>

#### <mark style="color:purple;">To ensure you have the latest version of the branch and to avoid potential merge conflicts:</mark>

```bash
git pull
```

### <mark style="color:yellow;">**Make Changes**</mark>

Edit, add, or delete your files based on the task at hand.

### <mark style="color:yellow;">**Stage Changes**</mark>

Before you commit, you need to stage your changes.&#x20;

This allows you to choose which changes you want to commit.

```bash
git add .
```

### <mark style="color:yellow;">**Commit Changes**</mark>

After staging, create a commit.&#x20;

Make sure your commit messages are descriptive and follow any commit message conventions your team might have.

```bash
git commit -m "Your descriptive message here"
```

### <mark style="color:yellow;">**Push Changes to GitLab**</mark>

#### <mark style="color:purple;">If it's the first time pushing the branch or if you haven't set the upstream branch before, you should use (can be any branch):</mark>

```bash
git push -u origin main
```

{% hint style="info" %}
The `-u` flag sets the upstream for your branch, meaning that next time you can simply run git push.&#x20;
{% endhint %}

#### <mark style="color:purple;">Otherwise, for regular pushes after setting upstream:</mark>

```bash
git push
```

## <mark style="color:red;">Integrating an Existing Folder with GitLab</mark>

#### <mark style="color:purple;">Navigate to your folder:</mark>

```bash
cd /path/to/your/folder
```

#### <mark style="color:purple;">Initialize Git:</mark>

```bash
git init
```

#### <mark style="color:purple;">Connect to your GitLab repository (SSH):</mark>

```bash
git remote add origin git@gitlab.com:yourusername/yourrepository.git
```

#### <mark style="color:purple;">Commit any existing changes:</mark>

```bash
git add .
git commit -m "Initial commit or a relevant message"
```

#### <mark style="color:purple;">Push to GitLab:</mark>

```bash
git push -u origin main
```

## <mark style="color:red;">Renaming Branch</mark>

#### <mark style="color:purple;">Rename your local branch:</mark>

```bash
git branch -m oldname newname
```

#### <mark style="color:purple;">Push the renamed branch to GitLab:</mark>

```bash
git push -u origin newname
```

#### <mark style="color:purple;">Update the default branch on GitLab:</mark>

* Go to your project.
* "Settings" > "Repository".
* Set new default branch.

## <mark style="color:red;">Dealing with Authentication Issues</mark>

### <mark style="color:yellow;">SSH Authentication</mark>

#### <mark style="color:purple;">Ensure you have set up SSH keys. If not:</mark>

```bash
ssh-keygen -t rsa -b 4096 -C "youremail@example.com"
cat ~/.ssh/id_rsa.pub
```

Copy and add the key to GitLab.

### <mark style="color:yellow;">Changing Remote URL to SSH</mark>

#### <mark style="color:purple;">If facing HTTP authentication issues:</mark>

```bash
git remote set-url origin git@gitlab.com:yourusername/yourrepository.git
```

## <mark style="color:red;">Using git blame</mark>

### <mark style="color:yellow;">Basic Usage</mark>

#### <mark style="color:purple;">View the blame for a file:</mark>

```bash
git blame filename.ext
```

#### <mark style="color:purple;">Specifying a Range:</mark>

```bash
git blame filename.ext -L 10,20
```

#### <mark style="color:purple;">Ignoring Whitespaces:</mark>

```bash
git blame -w filename.ext
```

#### <mark style="color:purple;">Viewing Changes Since a Specific Commit:</mark>

```bash
git blame filename.ext COMMIT_HASH^..
```

#### <mark style="color:purple;">Displaying More Detailed Information:</mark>

```bash
git blame -l filename.ext
```

## <mark style="color:red;">Advanced Git Commands & Tips</mark>

### <mark style="color:yellow;">Viewing Commit History</mark>

#### <mark style="color:purple;">To view the commit history:</mark>

```bash
git log
```

#### <mark style="color:purple;">For a more compact view with graphics:</mark>

```bash
git log --oneline --graph --all
```

### <mark style="color:yellow;">Stashing Changes</mark>

#### <mark style="color:purple;">If you've made changes that you're not ready to commit yet:</mark>

```bash
git stash
```

#### <mark style="color:purple;">To apply stashed changes:</mark>

```bash
git stash apply
```

#### <mark style="color:purple;">To drop a stash:</mark>

```bash
git stash drop
```

### <mark style="color:yellow;">Checking Out Branches or Commits</mark>

#### <mark style="color:purple;">To checkout a specific branch:</mark>

```bash
git checkout branch_name
```

#### <mark style="color:purple;">To checkout a specific commit:</mark>

```bash
git checkout commit_hash
```

### <mark style="color:yellow;">Reverting Changes</mark>

#### <mark style="color:purple;">To revert a specific commit:</mark>

```bash
git revert commit_hash
```

### <mark style="color:yellow;">Resetting Changes</mark>

#### <mark style="color:purple;">To reset your branch to look exactly like another branch (useful for syncing with</mark> <mark style="color:purple;"></mark><mark style="color:purple;">`main`</mark> <mark style="color:purple;"></mark><mark style="color:purple;">or</mark> <mark style="color:purple;"></mark><mark style="color:purple;">`master`</mark><mark style="color:purple;">):</mark>

```bash
git reset --hard branch_name
```

{% hint style="warning" %}
This discards all changes in your current branch that don't exist in the branch you're resetting to.
{% endhint %}

### <mark style="color:yellow;">Handling Merge Conflicts</mark>

#### <mark style="color:purple;">After merging, if there are conflicts:</mark>

1. Open the conflicted files and resolve the conflicts.

#### <mark style="color:purple;">After resolving:</mark>

```bash
git add resolved_file.ext
git commit
```

### <mark style="color:yellow;">Creating and Deleting Branches</mark>

#### <mark style="color:purple;">To create a new branch:</mark>

```bash
git branch new_branch_name
```

#### <mark style="color:purple;">To delete a branch:</mark>

```bash
git branch -d branch_name
```

### <mark style="color:yellow;">Fetching and Merging</mark>

#### <mark style="color:purple;">To fetch updates from the remote without merging:</mark>

```bash
git fetch
```

#### <mark style="color:purple;">To merge a specific branch into your current branch:</mark>

```bash
git merge branch_name
```

### <mark style="color:yellow;">Tagging</mark>

#### <mark style="color:purple;">To tag a specific commit (useful for versioning):</mark>

```bash
git tag v1.0 commit_hash
```

### <mark style="color:yellow;">Searching</mark>

#### <mark style="color:purple;">To search the commit logs:</mark>

```bash
git log --grep="search_term"
```

#### <mark style="color:purple;">To search for a specific term in your code:</mark>

```bash
git grep "search_term"
```
