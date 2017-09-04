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

### Import Existing Repository to a Server

We have an existing local Git repository, and want to copy it to a remote server. The repository on a server is a **bare repository, one without working directory**. 

When a remote server is maintained by you instead of Github, you may need to create a bare repository. A **bare repository is a Git repository without working directory**. 

1. `cd` to the **parent level** of local Git repository. 

2. Run `git clone --bare localrepo barerepo.git`. A bare repo named `barerepo.git` is created as a clone of local project. It contains files in a regular `.git` directory of `localrepo `, but no working directory. 

3. Copy the bare repository onto server, `scp -r barerepo.git <server url>:<path>/barerepo.git`.

4. **Do another clone** is an easy way to make local directory track the remote one. Run `git clone <server url>:<path>/barerepo.git localproj`. 

5. On server, `cd` to `<path>/bareproject.git` and run `git init --bare --shared` to **set proper write permission** to the repo on server.

### Create New Git Repository on a Server

We want to create a new empty Git repository on a server.

1. On server, `cd` to the **parent level** of project root. 
2. Run `git init --bare barerepo.git`. A Git directory named `bareproject.git` is created. 
3. On local machine, Run `git clone <server url>:<path>/barerepo.git localrepo`. 


## Track Changes{#basic-track}

### Stage Files

Use `git add <file>` to add the file to staging area, which **tracks new file or stage modified file**. Note it means "add this content to the next commit". 

Note file in staging area is a snapshot of the exact state of the file when `git add` is executed. The the file is modified after `git add`, it can be in both staged and modified state. We need to execute `git add` again to add the latest change to staging area. 

### Commit Files

Use `git commit` to create a snapshot for **everything in the staging area**. 

- `git commit -m "comment"`: type commit message inline. 
- `git commit -am "comment"`: stage all modified files, then commit. Note it does not work with untracked files. 

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

### Redo Commits

Redo commit if you forget to add some files, or need to change commit message. 

- `git commit --amend` **merges current staging area to last commit**, and give you a chance to change the commit message. Note last commit is replaced with amended commit. 

### Unstage a File

Unstage a file moves the file out of staging area **back to its original untracked or modified** state. 

- `git reset HEAD -- <file>` to unstage. 
- `git config --global alias.unstage 'reset HEAD --' ` to add an alias.  

### Unmodify a File

Unmodify a file discards all changes to a modified file, and use the **snapshot from last commit to copy over** the file. 

- `git checkout -- <file>` to check out content into the working directory.
- Note the **double dash `--` marks the end of option**, thus separates the git object from file path. For instance it separates a file named `master` and a branch named `master`. 
- `git config --global alias.unmodify 'checkout --'` to add an alias. 
- Note if the file is not in the last commit, the command fails. 