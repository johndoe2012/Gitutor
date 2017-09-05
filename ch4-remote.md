# Git Remote Management

## Remote Repository {#remote-repo}

Git remote repository is maintained by multiple collaborators, therefore usually hosted on the Internet. Status of remote can be different from local repository of individual collaborator, and needs to be synced from time to time. 

- `git remote -v`: list all the remote repos currently associated with local repo. It shows the short name, url, and permission as fetch or push. 
- `git remote show <shortname>`: inspect specified remote repo. It shows all the branches it has and they are tracked or not. 
- `git remote add <shortname> <url>`: add a new remote repo to local repo. 
- `git remote rename <oldshortname> <newshortname>`: rename a remote repo.
- `git remote remove <shortname>`: remove a remote repo. 

### Import Existing Repository to a Server

We have an existing local Git repository, and want to copy it to a remote server. The repository on a server is a **bare repository, one without working directory**. 

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

## Remote Tracking Branch {#remote-branch}

Branches of a remote repo is called remote branch. However what is interesting is remote tracking branch. 

**Remote tracking branch** is a local reference to the state of remote branch, including all the files, commits, branches, etc. It usually takes the form of `<remote repo>/<remote branch>`, such as `origin/master`. You **cannot modify it except for syncing** it with the remote branch. 

Your local repo can **set up numerous remote tracking branches** with different remote repo on the same project. 

## Remote Branch Synchronization {#remote-sync}

### Clone

`git clone` is a special case of synchronization. It will automatically do the following: 

- create local repo and add a remote repo called `origin`
- set up a remote tracking branch called  `origin/master`
- create a local branch called `master` which points to the same commit as `origin/master`. 

![remote branch clone](./res/remote-branches-clone.png)

### Fetch

When you do some work on your local `master`, and someone pushed changes to `origin` that advanced remote `master`. **The remote `master` and remote tracking `origin/master` becomes out of sync**. 

Use `git fetch origin` to synchronize the remote tracking branch. 

![remote branch fetch](./res/remote-branches-fetch.png)

Use `git fetch teamone` when there are multiple remote repositories and remote branches. 

![remote branch fetch multiple](./res/remote-branches-fetch2.png)

Note when you do a fetch that brings down new remote tracking branches, you do **NOT automatically have a local editable copies** of them, although `master` is a special case. For instance you fetched `origin/hotfix` and want to extend your work based on it, you have two options: 

- `git merge origin/hotfix master`: merge `origin/hotfix` into your work then making further changes. 
- `git checkout -b hotfix origin/hotfix`: create a new local branch `hotfix` that starts where `origin/hotfix` is. 

### Push

When local work is done then you want to update it to the remote repository. This requires you to explicitly push to remote repository to synchronize your work. 

- Use `git push origin master` to sync local `master` branch with remote branch with the same name on repo `origin`. 
- Use `git push origin master:remotemaster` to sync with remote branch of different name. 
- Before each push, it is **good idea to do a fetch first, and merge** local branch with latest remote tracking branch. 

### Track

To conveniently use `git push` and `git fetch`, it is best to link local branch directly with remote tracking branch. 

- When a local branch is created with `git checkout -b <localbr> <shortname>/<remotebr>`, it automatically tracks the remote branch. 
- If local branch already exists, use `git branch -u <shortname>/<remotebr>` to set up an upstream tracking. 

Use `git branch -vv` to see local branches with tracking information. 

### Delete

Use `git push <shortname> --delete <remotebr>` to delete a remote branch. 