Git recipes

| Action you want to do      |   Git command   |  Equivalent to|
| ------------- |:-------------:| ---:| 
|Show you your current branch and changed files     | git status| |
|See branches                                       | git branch||  
|Add all modified files (to the future commit == staging area) |git add .||
|Add a specific file(s) (to the future commit == staging area) |git add _<path-to-file>/FILENAME_ <p/>git add _FILENAME_ _FILENAME_ [...] ||
|commit|-||
|Add a comment to the files added |git commit -m "_MESSAGE_"||
|Add all files and make a commit at the same time|git commit -am "_MESSAGE_" ||
|fix a message of a commit I just made|  git commit --amend -m "_NEW_MESSAGE_" ||
|Display the history of commits (all modifications made on the project)|git log||
|Display the history of commits filtering using grep |git log --grep="Refactor"||
|See last changes on a file since the last commit | git diff _FILENAME_ ||
|tags | -| 
|Add a tag to the commit|git tag 1.3.1||
|Push the commit with the tag |git push -u origin master --tags||
|Show last commit, with files changed (diff)|git show||
| checkout | -| 
|create and move to branch |git checkout -b hotfix/branch1  |  git branch hotfix/branch1  <p/> git checkout hotfix/branch1 |
|work from a remote branch to local branch |  git checkout -b feature/branch2 <p/> git pull origin feature/branch2||
|Discard the changes to one file in the repo|git checkout -- path/to/the/file.txt||
|rollback a file to a certain commit|git checkout <commit_ID> path/to/the/file.txt||
||||
| branch | -| |
|show all Remote branch|git branch -r||
|show all Local branch|git branch||
|All local and remote branch|git branch -a ||
|See all local branches & last commit comment|git branch -v||
|Delete local branch hotfix/branch4|git branch -d hotfix/branch4      ||
| push | -| |
|Push to master branch|git push -u origin master    | |
|Push your branch to the remote repository     |git push -u origin feature/branch3||
|Push your branch to the remote repository & include tags|git push -u origin feature/branch3  --tags|
|Delete remote branch hotfix/branch4|git push origin --delete hotfix/branch4     |git push --delete origin tagname| 
| pull | -| |
||||
| merge | -| |
||||
| squash | -| |
||||
| rebase | -| |
||||
| reset | -| |
|Local, undo all the changes introduced since the last commit|git reset --hard||
|you have added (staging) file3.txt, but not committed; you want to revert this addition|git reset HEAD file3.txt |git rm --cached file3.txt |
| init | -| |
| initilyze git repo | git init | |
|revert|||
|Revert changes introduced by a specific commit |  git revert <commit_ID>||
|rebase|||
|make a commit completely disappear from history|git rebase -i [revision number]||

### Recipe for removing a file added 5 commits previous

1.Create and check out a temporary branch at the location of the bad merge

**git checkout -b tmpfix <sha1-of-merge>**

2.Remove the incorrectly added file

**git rm somefile.orig**

3.Commit the amended merge

**git commit --amend**

4.Go back to the master branch

**git checkout master**

5.Replant the master branch onto the corrected merge

**git rebase tmpfix**

6.Delete the temporary branch

**git branch -d tmpfix**

### Git force pull to overwrite local files (saving your committed local changes to a tmp branch)
1. create a new branch based on your current local branch

**git branch new-branch-to-save-current-commits**

2.git fetch downloads the latest from remote without trying to merge or rebase anything.

**git fetch --all**

3.resets the master branch to what you just fetched. The --hard option changes all the files in your working tree to match the files in origin/master

**git reset --hard origin/master**

4.Update branch
**git pull origin master**