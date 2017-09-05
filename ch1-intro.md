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

## Git Objects {#intro-object}

Git is basically a **key-value data store**. Every time we stage some files and make a commit, three types of objects are created:

- it takes **snapshot of each file's content**, called `blob` object. A hash key is assigned to each `blob` object, and the `blob` is stored in a directory named by the key. Note `blob` is not exactly the file, but a snapshot of content. 

- it creates a `tree` object to keep track of mapping between hash key and actual **filenames in the file system**. 

- it creates a `commit` object with **meta data and a pointer to the tree**.

  ![A commit and its tree](./res/objects.png)

When consecutive commits are made, common object includes **a pointer to its parent commit object**. 

![Commits and their parents](./res/commit-objects.png)

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

## Ignore Files {#intro-ignore}

Add files to `.gitignore` file to prevent Git from automatically adding them or showing them as untracked. 

- Files already tracked by Git are not affected. 
- `.gitignore` should be **in working directory**, not the `.git` directory. 

Rules for the `.gitignore` file are: 

- Blank lines or lines starting with `#` are ignored.
- Glob pattern
  - `*` matches zero or more characters, `?` matches single character. 
  - `[abc]` matches `a` or `b` or `c`.
  - `[0-9]` matches range 0 to 9.
  - `**` matches nested directory: `a/**/z` would match `a/z`, `a/b/z`, `a/b/c/z`
- End pattern with `/` to specify directory.
- Start pattern with `!` to negate it. 