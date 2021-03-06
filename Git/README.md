# GIT HELP
#### Create new branch based on sprint-task-name
````
git checkout -b 1803-9342-terraform-provisioning
````
#### Add all files to new repo
````
git add -A ( ALL)
````
#### Add files interactively
````
git add -i ( Interactive )
````
#### Commit files with a message
````
git commit -m "commit message"
git commit -am "commit message" # Add files and commit
````

#### Push local branch to remote origin
````
git push origin 1803-9342-terraform-provisioning
````

#### Delete remote branch
````
git push origin :1803-9342-terraform-provisioning
````
#### Discard local commits and get remote origin state
````
git reset --hard origin/master
````
#### List branches
````
git branch -a # List local
git branch -r # List remote
````
#### Switch branches
````
git checkout <branch-name>
````
#### Set local branch to track remote branch
````
git checkout --track origin/dev
````
#### Revert to specific commit and push to master
````
git checkout <commit-id>
git rebase
git commit
git push origin master
````

