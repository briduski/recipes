
## Recipe #1  for removing a file added 5 commits previous

1. Create and check out a temporary branch at the location of the bad merge
> git checkout -b tmpfix SHA1

2. Remove the incorrectly added file
> git rm filename

3. Commit the amended merge
> git commit --amend

4. Go back to the master branch
> git checkout master

5. Replant the master branch onto the corrected merge
> git rebase tmpfix

6. Delete the temporary branch
> git branch -d tmpfix

## Recipe #2 Git force pull to overwrite local files (saving your committed local changes to a tmp branch)

1. Create a new branch based on your current local branch
> git branch new-branch-to-save-current-commits

2. Fetch downloads the latest from remote without trying to merge or rebase anything.
> git fetch --all

3. Reset the master branch to what you just fetched. The --hard option changes all the files in your working tree to match the files in origin/master
> git reset --hard origin/master**

4. Update branch
> git pull origin master**

git fetch -v && git merge FETCH_HEAD  ????

## Recipe #3 Overwrite local files (you miss data)
1. git reset --hard HEAD
2. git pull

**OR**

1. git fetch origin master
2. git merge -s recursive -X theirs origin/master

## Recipe #4 Add files to the last commit
1. git commit -m 'initial commit'
2. git add forgotten_file
3. git commit --amend

## Recipe #5 Remove files staged
1. git add file1.txt  (staged)
2. git reset HEAD file1.txt

## Recipe #6 Remove last commit
1. git reset --hard HEAD^

## Recipe #7 Remove last commit - remote
1. git reset --hard HEAD^
2. git push origin -f

  
## Recipe #8 Change author of the very last commit
1. git commit --amend --author="Blabla <blabla@gmail.com>"
2. update de author config (in case you want also update from now author of the future commits):
    > git config user.name "Blabla" 
3. save the commit message (of the last - previous commit, you are able to change it )

## Recipe #9 Change author of all commits in a repo (all branches)
1. open the script change-author.sh, edit old_email, new_email, name
2. run script
3. git push --force --tags origin 'refs/heads/*'

#https://help.github.com/en/github/using-git/changing-author-info

## Recipe #10 Commits (#3) on master, but they were premature to be in the master branch
1. git branch topic/wip
2. git reset --hard HEAD~3 
3. a. git checkout topic/wip (in case you want to continue here)
3. b. (on master) git push origin master -f [FORCE TO UPDATE REMOTE]

## Recipe #11 Unstage a staged file
1. git add file.rar
2. git reset HEAD file.rar

## Recipe #12 Unstage a staged file, you have just added to .gitignore
1. git add file.rar
2. ==> add to git .gitignore file "*.rar"
3. Clean cache: git rm --cached file.rar

## Recipe #13 Initialize a git repo locally and add a remote url associated to it
1. mkdir repo_name
2. cd repo_name
3. git init
4. git add Anyfile.txt
5. git remote add origin  https://github.com/.git_account./repo_name.git
6. git push -u origin master


## Undo changes in a single file modified only in working directory
git checkout -- filename.txt

OR
2020  => git restore filename.txt

https://docs.gitlab.com/ee/topics/git/numerous_undo_possibilities_in_git/