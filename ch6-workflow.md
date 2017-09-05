# Distributed Workflow

## Commit Guideline {#wf-guide}

1. Use `git diff --check` to avoid **whitespace error**.
2. Make each commit a logically separate and digestible changeset. 
3. Commit message: 
   - single line of 50 character summary. 
   - a blank line. 
   - detail explanation with 72 character per line. 
   - use imperative present tense such as ''add tests". 

## Small Private Team {#wf-small}

In a small private team developers are equal and all have rights to push to the public repo. Merges are done client-side at commit time before pushed to server. 

The typical workflow is:

- **clone** the repo to local.
- create topic **branch** and work on it for a while. 
- **merge** the topic branch to local master branch. 
- **fetch** `origin/master`, then **merge** it into local `master`.
- **test** the code still working properly. 
- **push** to `origin/master`. 

## Managed Private Team {#wf-managed}

In a larger managed team we have small groups working in parallel on different features, and a manager that integrates group-based contributions. All groups have rights to create topic branches on the repo, but only the manager has rights to merge the topic branches to repo `master`. 

The typical workflow is following: 

- **clone** the repo to local.
- create a topic **branch** `featureA`, and work on it for a while. 
- **push** `featureA` to `origin` by creating a new remote topic **branch** `origin/featureA`. 
- **inform** group members about the addition. 
- create a new topic **branch** `featureB` based on `origin/master`, and work on it for a while. 
- be notified that changes about feature B has been made on `origin/featureBee`. 
- **fetch** `origin` to check out the changes. 
- **merge** changes from `origin/featureBee` onto our local work on `featureB`. 
- push to `origin/featureBee` and make `featureB` tracks `origin/featureBee`. 
- inform manager to merge `origin/featureA` and `origin/featureBee` to `origin/master`. 
- fetch `origin/master`. merge it to local `master`. 

## Forked Public Project {#wf-forked}

When contributing to public project, you do not have the permission to push or create branches on remote repo. 

The typical workflow is following: 

- **clone** the repo to local. 
- create a topic **branch**, and work on it for a while. 
- **fork** the project on hosting site to create your own writable fork of the project. 
- **add** the fork repo as second **remote** (first is the origin repo you cannot write). 
- **push** the topic branch to fork repo. Note **do not merge** to `master` then push `master`. 
- initiate a **pull request** to the maintainer. 