# Basic Operations

## File Status {#basic-status}

A file, with respect to Git system, has one of the four status: 

- untracked: newly created file that has not been in any snapshot nor the staging area
- committed: file included in last snapshot
- modified: file changed since last snapshot but has not been staged yet
- staged: modified file marked to be included in next snapshot

![File status](./res/lifecycle.png)

Use `git status` to check status of files in working directory. 

  ```shell
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   README2

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   README

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        SUMMARY
  ```
Use `git status -s` to check short status of files. There are two columns for the short status. Left is for staging area, right is for working directory. 

```shell
$ git status -s
 M README            # modified, but not staged
M  lib/simplegit.rb	 # modified, then staged
MM Rakefile          # modified, then staged, then modified again
A  lib/git.rb        # new file, then staged
AM lib/repo.rb       # new file, then staged, then modified again
?? LICENSE.txt       # untracked
```

## Git Repository {#basic-repo}

There are three main sections of a Git repository:

- Git directory: usually a `.git` directory in project root, where Git stores the metadata and object database for the project. 
- Working directory: the project root, where last version of files are extracted to for further modification. 
- Staging area: a index file inside Git directory, which records all files ready to be included in the next commit. 

### Import Existing Directory

We have an existing project directory and would like to use Git to manage it. 

1. `cd` into the existing project root.
2. Run `git init`. A `.git` directory is created. 

Note **nothing has been tracked by Git yet**, for which we need to make an initial commit. Also Git ignores empty directory. **Create at least one single file for tracking**. 

### Clone Existing Repository

We have a Git repository somewhere and want to create a local working directory of it. 

1. `cd` to the **parent level** of project root. 
2. Run `git clone <repo URL> <local name>`. A directory name `<local name>` is created. It contains a working copy of all project files, as well as complete Git database inside a `.git` directory. 


Note all files are **already been tracked, and unmodified**. 

`git clone` supports `https`, `ssh`, and `git` protocols. 

## Track Changes{#basic-track}

### Stage Files

Use `git add <file>` to add the file to staging area, which **tracks new file or stage modified file**. Note it means "add this content to the next commit". 

Note file in staging area is a snapshot of the exact state of the file when `git add` is executed. The the file is modified after `git add`, it can be in both staged and modified state. We need to execute `git add` again to add the latest change to staging area. 

### Commit Files

Use `git commit` to create a snapshot for **everything in the staging area**. 

- `git commit -m "comment"`: type commit message inline. 
- `git commit -am "comment"`: stage all modified files, then commit. Note it does not work with untracked files. 

- `git commit --amend` **appends current staging area to last commit**, and give you a chance to **change the commit message**. Note last commit is replaced by amended commit. Note redo commit may cause issue if it is done after a `git push`. 

### Stash Files

When we are working on a project in a messy state, but need to switch branch or make a merge, we need to put away the work temporarily. 

Use `git stash` to put the work into a stack, then you will have a clean working directory. 

After you come back to continue the work, use `git stash pop` to retrieve the previous work and remove it from the stack. 

```shell
$ git stash
$ git stash list
stash@{0}: WIP on master: 049d078 added the index file
stash@{1}: WIP on master: c264051 Revert "added file_size"
stash@{2}: WIP on master: 21d80a5 added number to log
$ git stash pop
```

### Remove Files

When we `rm` a file in the file system, the file is only put into **modified but unstaged** state. To make Git reflect on this change, we still need to stage it and commit. 

- `git rm <file>` **removes** the file from file system, then stage the change automatically. 
- `git rm --cached <file>` **untracks** a file from git, but keep it in the file system. 

Note we still need to **commit the removal**. 

### Rename Files

When we `mv` a file in the file system, the file is put into **modified but unstaged state while a new untracked file is created**. 

Use `git mv <old> <new>` to facilitate the process. It is equivalent to three commands. 

```shell
$ mv README2 README
$ git rm README2
$ git add README
```

Still, remember to commit the change. 

## Undo Changes {#basic-undo}

### Syntax: `--` , `~`, and `^`

`git checkout -- <file>`. The **double dash `--` marks the end of option**, thus separates the Git object such as commit and branch, from file path. For instance it separates a file named `master` and a branch named `master`. 

`git revert HEAD~3`. The tilde `~n` means n commits from current `HEAD` commit. 

`git revert HEAD^2`. The caret `^n` means nth parent of current `HEAD` commit if it is a merge commit. 

![tilde and caret](./res/tilde-caret.svg)

### Unstage a File

Unstage a file moves the file out of staging area **back to its original untracked or modified** state. 

- `git reset HEAD -- <file>` to unstage. 
- `git config --global alias.unstage 'reset HEAD --' `.  

### Unmodify a File

Unmodify a file discards all changes to a modified file, and use the **snapshot from last commit to copy over** the file. 

- `git checkout HEAD -- <file>` to check out specified file content from last commit into the working directory.
- `git checkout HEAD~2 -- <file>` to check out specified file content in third last commit into the working directory. 
- `git config --global alias.unmodify 'checkout HEAD --'`. 
- Note if the file is not in the previous commit, the command fails. 

### Revert Commits

We want to start a **new branch from a previous commit**, which require us to move the `HEAD` pointer to an early commit. 

- `git checkout HEAD~2` to restore entire working directory to third last commit. 
- Note the `HEAD` does not point to any branch at the moment which is dangerous. From here we **should make a new branch immediately**. 

We have come changes that has already been committed, but we want to **discard these committed changes**. Git make the revert by find out the changes in those commit, recover the state of working directory before the changes, then **make a new commit for the reverted state**. 

- `git revert HEAD~3`: Revert the changes specified by the fourth last commit in current branch, and create a new commit with the reverted changes.
- `git revert topic~5..topic~2`: Revert the changes done by commits range from the fifth last commit in topic(exclusive), to the third last commit in topic (inclusive).