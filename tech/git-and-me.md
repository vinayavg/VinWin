---------------------------------------------------------------------------------------

git clone <url>
  
cd repoName

vi ... blah blah blah

git status

git add .   # or needy relative path of selective file

git commit -m "sweet message to appear on history of commits"

git push origin HEAD:refs/heads/master  # Magic ref with Gerr : git push origin HEAD:refs/for/master    # needy branch basically


git reset hard origin/needyBranch

git clean -fd


git fetch ; git rebase


---------------------------------------------------------------------------------------

Some from Github perspective, but underlying Git commands being the same


git init projName   # to get started

--- .git folder : 

git remote add origin https://github.com/...

cat .git/config

git-scm.com

git config --global user.name 
user.email

git config --global core.autocrlf true

git config --local user.name blahBlah

#Craft commits!

git diff             # differs from staging area

git diff --staged    # diffs from last commit & staging area

git diff HEAD 	     #  

git diff --color-diffs

git diff --word-diffs

git diff --stat    # just stat

git log   # from latest to oldest ( chronlogical ) 40 hex ref 

git log --oneline

git log --stat   # what files were involved in the commit listed out

git log --patch  # diff.s b/w each of subsequent commits

git log --patch --oneline

git log --graph --all --decorate --oneline  # commit structure

git rm fileName   # both removes & stages file for commit

git add -u .  # all deletions from current dir onwards to stage from all rm*

git rm --cached fileName  # untracked file but still stage file for 

git mv oldPath/fileName newPath/fileName # renaming/moving is same thing in git

git add -A .  # deletes moved files & handles renames

git log --stat -- filePaht

git log --stat --M --follow -- filePaht  # moved file history as well

touch .gitignore
.project
.classpath
*.log
tmp/

explicitly using it
!audit log

git ls-files --others --ignored --exclude-standard    # to get above list


git branch newBranchName # to create

git branch  # to list

git branch -d someBranchName

git checkout someBranchName  # re-write working tree

git checkout commitRef  # Detached head state.. to look at that point of time

git checkout someBranchName  # to get back to required branch

git checkout -- someFile # write content from last commit

git checkout -b someBranchName #  create & switch


git checkout someNeedybranch

git merge fromRequiredBranch

open conflicted files ; remove conflicted lines 

git add ; git commit

git merge --abort   # abondon for timebeing

git merge --squash targetBranch  # bring from needy branch & commit


git remote add origin url://
git remote set-url origin newUrl
git remote rm origin
git remote -v


git branch -r  # remote branches

git fetch origin 

git pull  origin   # fetch + merge

git push origin


---------------------------------------------------------------------------------------

