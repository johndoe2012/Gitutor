# Introduction

## Getting Help {#intro-help}

Use `git help <verb>` to open the man page. 

## Version Control Systems {#intro-vcs}

Version control is a system that records changes to files over time so that you can recall specific versions later. It allows you to: 

- revert files or entire project back to a previous state
- compare changes over time
- see ownership of changes

### Local Version Control Systems

A local version database is used to keep track of every revision. 

- Difference is formatted as patches
- Hard to support collaboration
- `rcs`

### Centralized Version Control Systems

A version database is kept at central version for everyone to check out the revisions.

- Easy to manage
- Single point of failure
- `CVS`, `Subversion`, `Perforce`

### Distributed Version Control Systems

Everyone gets a complete version database by fully mirroring the repository. 

- Allow collaboration with different groups within the same project
- New types of workflow
- `Git`, `Mercurial` 

## Git Features {#intro-feature}

### Snapshot, Not Differences

This is the major difference between `Git` and other VCS. 

- Other VCS stores revisions as **changes made to files** over time. 
- `Git` takes **snapshot of all files** at the time, and revision is a reference to the snapshot. If a file is not changed, a symbolic link is created instead of a copy. 

### Nearly Every Operation is Local

Copy of repository is kept locally so that operations can access and modify the copy easily and fast. To synchronize the local copy with remote copies, user needs to manually ask `git` to do so. 

### Git has Integrity

Everything in `git` is check-summed with length 40 SHA-1 hash before it is stored, then can be referred by the checksum. 

### Git Generally Only Adds Data

Nearly all operations in Git adds data to the database, so that it is hard to lose data. 

## Configuration {#intro-config}

We can use `git config` tool to set and get the `Git` variable. The variables are stored in three files: 

- System-wise: variable stored in `/etc/gitconfig`. Use `git config --system` to set. 
- User-specific: variable stored in `~/.gitconfig`. Use `git config --global` to set.
- Repository-specific: variable stored in `<proj-root>/.git/config`. Use `git config` to set.
- Each level overrides same settings at higher level.

### Set Identity

You **MUST set `user.name` and `user.email`** before use.

```bash
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```

### Set Editor

Change default editor for `git`.

```shell
$ git config --global core.editor emacs
```

### Set Alias

Use `git config --global alias.<newcommand> '<old command strings>'` to create alias for old command. 

- `git config --global alias.unstage 'reset HEAD --'`: add `unstage` command. 
- `git config --global alias.unmodify 'checkout --'`: add `unmodify` command.
- `git config --global alias.last 'log -1 HEAD'`: add `last` command.

### Check Configuration

Use `git config --list` to check current configurations. 