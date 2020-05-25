# Git Tips


* [Basics](#basics)
* [.gitignore](#gitignore)
* [Log](#log)
* [Reverting](#reverting)
* [Remotes](#remotes)
* [Tags](#tags)
* [Branching](#branching)
* [Deleting a repository](#deleting-a-repository)
* [References](#references)

## Basics

1. Git - more like a mini filesystem - thinks about its data more like a stream of snapshots, unlike other delta-based version control systems (Subversion.)

2. Unlike CVCS, nearly every operation in Git is local.

3. Git has integrity. Everything is checksummed. It uses SHA-1 to store everything in it's DB.

4. Git generally only adds data.

5. Git has three stages: *modified*, *staged*, and *committed*. And it has three main sections:
    * *The working tree*: a single checkout of one version of the project. Files are pulled out of the compressed DB in the Git directory and placed on the disk for modifications.
    * *The staging area*: - aka index - a file that stores info about what will go into the next commit.
    * *The Git directory*: - .git - stores the metadata and object DB.

6. `git commit --amend`:

    ```bash
    git commit -m "Initial commit"
    git add forgotten_file
    git commit --amend
    ```

    Then we'll have one single commit.

## .gitignore

1. When we want to ignore certain files or folders, create a .gitignore file and add the file/folder name inside:

    ```text
    file.txt # a single file
    folder/ # a folder named `folder`
    *.txt # all txt files
    ```

2. If a file is already tracked, use `git rm --cached foo.txt` to remove it first from the git repo.

## Log

1. When we want to get a simpler version of the log, use `git log --pretty=oneline`.

## Reverting

1. Use `git checkout .` to revert to the last commit before adding the new changes.

2. Use `git checkout` plus the first 6 characters of the reference ID to check out the old commits. This enters the detached HEAD state. And it's best ***not to make changes*** when checking out old commits. Use `git checkout master` to go back the master branch.

3. Use `git reset --hard` plus the first 6 character of the reference ID to reset the project to the old commit.

4. `git reset HEAD README.md` to unstage the file.

5. `git checkout -- README.md` to discard the changes for the file.

## Remotes

1. `git remote -v` to see all the remotes verbosely with URLs. `origin` is the default name.

2. `git pull` = `git fetch` + `git merge`.

3. `git push <remote> <branch>`: `git push origin master`.

4. `git remote show <remote>` to see details of a remote.

5. `git remote rename <old_remote> <new_remote>` and `git remote remove <remote>`.

## Tags

1. Annotated tags: `git tag -a v1.0 -m "my first tag"`. `git tag` to see the tags

2. `git show v1.0` to show the details.

3. Lightweight tags: `git tag v1.0-lw` (just provide a tag name only). It's a commit checksum - no other info is kept.

4. Add tags for previous commits: `git tag -a v1.0 <log_number>`.

5. `git push` doesn't push tags to the remote servers. We need `git push origin <tag_name>` or `git push origin --tags` for all tags.

6. `git tag -d <tag_name>` and `git push origin --delete <tag_name>` for deletion.

## Branching

1. `git branch <branch_name>` creates a new branch. `git checkout <branch_name>` moves `HEAD` to the branch. The shorter version is `git checkout -b <branch_name>` to do both at the same time.

2. `git log --oneline --decorate` shows the branch pointers.

3. `git log --oneline --decorate --graph --all` shows the divergence.

4. Branches are cheap since they are essentially a file that has the 40-character checksum of the commit pointed to.

5. `git checkout master` + `git merge hotfix` will fast-forward (or use the recursive strategy) the master branch to the match the hotfix branch. Then `git branch -d hotfix` deletes it.

6. In case of merge conflict, we need to choose one side or merge the contents ourselves. Then add and commit the file again.

7. `git branch -v` shows the last commit of each branch. `git branch --merged` and `git branch --no-merged` shows the branched that are already merged to the current branch or not yet respectively. Or `git branch --no-merged master`.

8. `git checkout experiment` + `git rebase master` + `git checkout master` + `git merge experiment` gives a fast-forward merge.

9. `git stash` is a convenience tool. But since branching is cheap, we can always branch and then delete it.

10. `git branch -a` to see all the branches.

11. `git fetch` and `git checkout -b test remotes/origin/test` creates a local `test` branch and connects it with the remote branch.

## Deleting a repository

1. We can either delete the .git directory in a file browser or use `rm -rf .git`.

## References

* [Python Crash Course, 2nd Edition: A Hands-On, Project-Based Introduction to Programming](https://www.amazon.com/Python-Crash-Course-2nd-Edition/dp/1593279280/ref=sr_1_1?keywords=python+crash+course&qid=1558808134&s=gateway&sr=8-1)

* [Pro Git, 2nd Edition](https://git-scm.com/book/en/v2)
