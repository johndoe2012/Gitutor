# Git History

### View Changes {#history-diff}

Use `git diff` to view differences between any two `trees`, at **file level**. 

- `git diff <file>` shows difference between **working directory and staging area**. Note it does not show untracked files. 
- `git diff --cached <file>` shows difference between **staging area and last commit**. 
- `git diff HEAD HEAD~3` shows difference between **two specified commits**. 
- `@@-43,7 +2,8@@` means that following is a display of 7 lines starting line 43 in first file, and 8 lines starting line 2 in second file. 

### View History {#history-log}

Use `git log` to display the change history, at **commit level**.

- `git log -p`: show history with diff.

- `git log --pretty=oneline`: format information for each commit to one line. 

- `git log --pretty=format: "%h - %s"`: custom format information for each commit, showing hash and subject for instance.

- `git log --graph`: show a graphic history tree.

- `git log -2`: show last 2 commits. 

- `git log --since="2008-10-01" --until="2008-11-01"`: output commits within given range.

- `git log --author=gitster`: output commits by author name `gitster`. 

- `git log --no-merges -- test/`: pass a path as last option to `git log`, separated by `--` from other options, to output only commits making changes to the directory or file. 

- `git log --no-merges branchA..branchB`: only show commits that are on `branchB` but not on `branchA`. 

```shell
$ git log --pretty="%h - %s" --author=gitster \
	--since="2008-10-01" --before="2008-11-01" --no-merges -- t/
$ git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset%n' --abbrev-commit --date=relative --branches
```

### Tag Commits {#history-tag}

Use `git tag` to manipulate tags for commits.

- `git tag`: list all the tags in lexicographic order.
- `git tag -l "v10.0*"`: list tags matching specified pattern.
- `git tag -a v0.1 -m "version 0.1"`: create an annotation tag named "v0.1" with message "version 0.1".
- `git tag -a v1.0 -m "version 1.0" <hash>`: add a tag for specific commit given its hash. 
- `git show v0.1`: display information for tag `v0.1`. 
- `git push origin --tags`: note `git push` will not automatically push tags to remote server. add `--tags` options to do so. 
