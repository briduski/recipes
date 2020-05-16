Git recipes

- [Examples](examples.md)

- Shortcuts table:

| Action you want to do      |   Git command   |  Comment / Equivalent to|
| ------------- |:-------------:| ---:|
|Show you your current branch and changed files     | git status| |
|"" and the changed code     | git status -v| |
|ADD|||
|Add all modified files (to the future commit == staging area) |git add .||
|Add a specific file(s) (to the future commit == staging area) |git add path-to-file/filename <p/>git add filename1 filename2 ||
|LOG|||
|List the files modifed |git log --name-only --pretty=format: | git log --name-only ** git log --name-status|
|-----|git log --stat --pretty="format:" $(git rev-parse --abbrev-ref HEAD)||
|-----|git log --stat --pretty=short --graph||
|Shows the changes of a specifc file|git log --full-history -- path-to-file |git log --stat | grep filename|
|Shows all files deleted|git log --diff-filter=D --summary | sed -n -e '/^commit/h' -e '\:/:{' -e G -e 's/\ncommit \(.*\)/ \1/gp' -e }||
|Show the commit when a file was affected'|git rev-list -n 1 HEAD -- path-to-file||
|Get all the commits which have deleted files and the files deleted;|git log --diff-filter=D --summary||
|Display diff (code changes), introduced in the last commit (-1) |git log -p -1||
|1-line => every commit: SHA1 + comment |git log --oneline||
|Sort output sorted by author commit|git shortlog||
|See Author & amount of commits |git shortlog -n -s -e|-n:number , -s:summary, -e:email|
|Display ... |git log --pretty=format:"%h - %an, %ar : %s"||
|Display Graph *************|git log --oneline --decorate --graph --all|git log --oneline --decorate --graph master|
|Display ...|git log --pretty=format:"%h %s" --graph||
|Filtering using grep |git log --grep="Refactor"||
|Search in the code| git log -S"queue" --pretty=format:'%h %an %ad %s'||
|Show last commit|git show||
|Rename file already on git repo|git mv README.md README||
|Other options |  git log --stat / git log --numstat /--merges --no-merges||
||||
|COMMIT|-||
|commit to git repo with a comment about the commit|git commit -m "message"||
|add to stage & commit to git repo => |git commit -am "_MESSAGE_" | git add <p/> git commit -m "message"|
|fix a message of a commit I've just made|  git commit --amend -m "new-message" ||
|See last changes on a file since the last commit | git diff filename ||
|See what you have staged so far| git diff --cached||
||||
|TAGS | -|
|Add a tag to the last commit (light tag)|git tag 1.3.1||
|Add a tag to specific commit (annotated tag) |git tag -a v1.2 -m "new-tag"||
|Add a tag to specific commit |git tag -a v1.2 SHA1||
|See the details of an annotated tag|git tag -v v1.2||
|Push the commit with the tag |git push -v origin v1.5||
|Push the commit with all the tags |git push origin branch-name --tags||
|Create a branch based on tag |git checkout -b feature-3 v2.0.0||
|Delete tag working locally|git tag -d tag-name||
|Delete tag remote |git push origin :refs/tags/tag-name|git push origin --delete tag-name|
||||
|CHECKOUT | -|
|Move HEAD to specific branch |git checkout branch-name||
|create and move to branch |git checkout -b hotfix/branch1  |  git branch hotfix/branch1  <p/> git checkout hotfix/branch1 |
|create local branch based on remote branch| git checkout -b serverfix origin/serverfix| git checkout -b feature/branch2 <p/> git pull origin feature/branch2|
|create & connect local branch sf to remote branch serverfix|git checkout -b sf origin/serverfix||
|Discard the changes to one file in the repo|git checkout -- ppath-to-file/filename.txt||
|rollback a file to a certain commit|git checkout SHA1 path-to-file/filename.txt||
||||
|BRANCH | -| |
|Create new branch|git branch branch-name||
|Rename specific branch |git branch -m branch-name new-branch-name  | |
|Create a branch based on specific commit |git branch branch-name SHA1|git branch branch-name HEAD~3|
|Show all local and remote branch|git branch -a ||
|Show all Remote branch|git branch -r||
|Show all Local branch|git branch||
|See all local && remote branches & the comment of the last commit |git branch -vv||
|Delete local branch hotfix/branch4 (valid for merged branches)|git branch -d hotfix/branch4      ||
||||
|PUSH | -| |
|Push to remote repository master branch|git push origin master    | git push origin feature/branch3|
|Push your branch to the remote repository & include tags|git push origin feature/branch3  --tags|
|Delete remote branch|git push origin --delete branch-name     |git push origin -d  tagname|
|Rename a remote branch |git push origin :branch-name new-branch-name||
|Push the current branch and set the remote as upstream|git push --set-upstream origin branch-name|git push -u origin branch-name|
||||
|PULL | -| |
||git pull||
||||
|FECH | -| |
|Download all updates from remote repository write it into local repository|git fetch ||
||||
|MERGE | -| |
|fast-forward merge. Not new commits in the destination branch |dest_branch:> git merge branch-name ||
|3-way-merge. There are commits in the destination branch, no present in the branch to merge |git merge branch-name + solve conflicts + git add + git commit||
||||
|RESET | -| it modifies history|
|Discard changes from git-repo, but remaining in staging area and in working directory|git reset --soft SHA1||
|Discard changes from git-repo and staging area, but remaining in working directory|git reset SHA1||
|Discard changes totally|git reset --hard SHA1||
|Unstage changes|git reset HEAD file3.txt |git rm --cached file3.txt |
||||
|INIT | -| |
|Initialize git repo | git init | |
||||
|REVERT||it does not modify history|
|Revert/Undo changes introduced by a specific commit |  git revert SHA1||
||||
|REBASE|||
|Rebase master branch on feature branch and after merge it into master|>branch feature: git rebase master + git checkout master + git merge feature||
|Make a commit completely disappear from history|git rebase -i [revision number]||
|Squash (==join commits). Get SHA1 of the previous commit you want to start the squash| git rebase -i SHA1, after edit file: pick, replace 'second' pick->s :wq, commit msg | |
||||
|REMOTE|||
|See remote servers configured|git remote||
|Show url of remote server|git remote -v||
|Show remote servers and local branches 'connected' to remote branches|git remote show origin||
|Add remote server|git remote add origin https://github.com/briduski/github-ud-course.git||
|Update tracking status |git remote update origin --prune||
||||
|Low level git|||
|see the stage area |git ls-files -s ||
|see the content of a commit by reading its hash/commit_id |git cat-file -p SHA-1||
||||
|Configure |||
|user |  git config --global user.name "R, RMA" ||
|email|  git config --global user.email "email"||
||||
|ALIAS|||
|Setting alias & using alias: see last commit|git config --global alias.last 'log -1 HEAD'|git log -1 HEAD == git last|
|Remove alias |git config --global --unset alias.co|edit git config file: vim ~/.gitconfig /*/ vim ~/.bash_profile && source ~/.bashrc|
|-|git config --global alias.ci commit |git commit == git ci |
|-|git config --global alias.co checkout|git checkout == git co|
|-|git config --global alias.br branch |git branch == git br|
|-|git config --global alias.st status|git status == git st |
|-|git config --global alias.sg 'log --pretty=format:"%h %s" --graph'|simple graph => git sg -5|
|-|git config --global alias.sch 'log --pretty=format:"%h - %an, %ar : %s"'|simple commit history => git sch -2 |
|-|git config --global alias.pg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"|pretty graph => git pg -6|
||||
|BLAME|||
|See file changes per line: sha1/who/when/ts/#line/line of a file|git blame pom.xml||
|Blame limit lines interval|git blame -L 10,13 pom.xml||
|Blame original author, not the last author|git blame -C pom.xml||
||||
|RM|||
|Clear cache |git rm --cached filename||
|Clear entire Git cache|git rm -r --cached .|--don't do that!!! git unstage *|
|Ignore a directory => add it to .gitignore|||
||||
|CHERRY-PICK|||
|Insert a commit of branch into another branch, creating a new commit |git cherry-pick SHA1||
|"", affecting only to working directory, not creating a commit|git cherry-pick SHA1 --no-commit||
||||
|REFLOG|Operations stored in reflog by default 90 days||
|See the operations performed in the repo|git reflog||
|"" only on specific branch|git reflog show branch-name||
|-|-|-|
|STASH|||
|Save changes from working directory (and/or staging area) not committed, into the Stash |git stash||
|Returning back to working directory changes saved into Stash |git stash pop||
|Others|||
|Setting config properties|||
|Global property|git config —global user.name   "briduski"||
|Loal -speific-repo property|git config user.name   "briduski"||
|Getting config properties|||
|Global property|git config —global user.name ||
|Local -speific-repo property|git config user.name  ||
|See local repo config|cat .git/config  ||
|-|-|-|
|BISECT|||
|Used when you find out when introduced a bug in the code|Binary search algorithm|-|
||git bisect start||
|Indicates in this commit there's found an error|git bisect bad||
|... this commit has no errors|git bisect good SHA1||
|... this is bad|git bisect bad||
|...|git bisect bad||
|... here you know in which commit was introduced the bug|git bisect good||
||git bisect reset||


 Mermaid:
 https://mermaid-js.github.io/mermaid-live-editor/

[![](https://mermaid.ink/img/eyJjb2RlIjoic2VxdWVuY2VEaWFncmFtXG5cdFdvcmtpbmdfRGlyZWN0b3J5LT4-K1N0YWdlOiBnaXQgYWRkXG5cdFN0YWdlLT4-K0dpdF9SZXBvOiBnaXQgY29tbWl0XG5cdEdpdF9SZXBvLT4-K1JlbW90ZV9HaXRfcmVwbzogZ2l0IHB1c2hcblx0R2l0X1JlcG8tPj4rV29ya2luZ19EaXJlY3Rvcnk6IGdpdCBjaGVja291dFxuXHRSZW1vdGVfR2l0X3JlcG8tPj4rR2l0X1JlcG86Z2l0IGZldGNoXG5cdFJlbW90ZV9HaXRfcmVwby0-PitXb3JraW5nX0RpcmVjdG9yeTpnaXQgcHVsbCIsIm1lcm1haWQiOnsidGhlbWUiOiJkZWZhdWx0In0sInVwZGF0ZUVkaXRvciI6ZmFsc2V9)](https://mermaid-js.github.io/mermaid-live-editor/#/edit/eyJjb2RlIjoic2VxdWVuY2VEaWFncmFtXG5cdFdvcmtpbmdfRGlyZWN0b3J5LT4-K1N0YWdlOiBnaXQgYWRkXG5cdFN0YWdlLT4-K0dpdF9SZXBvOiBnaXQgY29tbWl0XG5cdEdpdF9SZXBvLT4-K1JlbW90ZV9HaXRfcmVwbzogZ2l0IHB1c2hcblx0R2l0X1JlcG8tPj4rV29ya2luZ19EaXJlY3Rvcnk6IGdpdCBjaGVja291dFxuXHRSZW1vdGVfR2l0X3JlcG8tPj4rR2l0X1JlcG86Z2l0IGZldGNoXG5cdFJlbW90ZV9HaXRfcmVwby0-PitXb3JraW5nX0RpcmVjdG9yeTpnaXQgcHVsbCIsIm1lcm1haWQiOnsidGhlbWUiOiJkZWZhdWx0In0sInVwZGF0ZUVkaXRvciI6ZmFsc2V9)
