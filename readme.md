# I ain't afraid of no rebasing - Tutorials

Welcome to this set of tutorial sessions!

In these sessions we're going to be learning about Git rebasing, these are self-learning tutorials which I hope you
find useful and demystify rebasing. If you have any suggestions on how I can improve these tutorials, then please let me know. 

Happy learning! 

## Prerequisites

 1. Install [GIT](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) command line
 2. Clone this repository 
 ```git clone git@github.com:OAFCROB/tutorials.git```
 
## Tutorials 
As these tutorials rely heavily on the git history, we'll be working with various git branches for each of these tutorials.

### 1. Merging strategy

 1. Create a new branch `git checkout -b merge` this will represent our main/master branch.
 2. Create a new file to work with `echo "Merge commit 1" > example.txt`
 3. Add this file and commit to GIT `git add . && git commit -m "TUT-1: Merge commit 1"`
 4. Create a new feature branch `git checkout -b merge-feature` this will represent our feature branch.
 5. In the feature branch update the `example.txt` file to include `Feature commit 1`. Once that's done add the file and commit `git add . && git commit -m "TUT-1: Feature commit 1"`
 6. Checkout the merge branch, add a new line to the example.txt and commit it. This will represent another commit being added to our main branch. 
`git checkout merge`
`echo "Merge commit 2" >> example.txt`
`git add . && git commit -m "TUT-1: Merge commit 2"`
7. Once again checkout our feature branch, add a new line to the example.txt and commit it. This will represent the fact that we're actively working on this particular feature whilst the main branch is still being worked on.
`git checkout merge-feature`
`echo "Feature commit 2" >> example.txt`
`git add . && git commit -m "TUT-1: Feature commit 2"`
8. For good measure, lets add another line to the example file and commit that. 
`echo "Feature commit 3" >> example.txt`
`git add . && git commit -m "TUT-1: Feature commit 3"`
9. We'll now merge in the main branch, `merge` into the feature branch to represent us updating the feature branch with the latest from the main branch ready for a pull request. We are expecting a conflict here, accept both changes and add as a new commit.  
`git merge merge` 
```// Resolve the conflicts in your IDE
git add. && git commit -m "TUT-1: Feature commit 4 resolve merge conflict when merging in main branch"
```
10. Now let's look at the git history for what we've just done `git log`. You should see that all the commits are in chronological order.
11. Now let's see the commits in a slightly different way `git log --graph --decorate --pretty=oneline --abbrev-commit` 

### 2. Rebasing strategy 

 1. Create a new branch `git checkout -b rebase` this will represent our main/master branch.
 2. Create a new file to work with `echo "Rebase commit 1" > example.txt`
 3. Add this file and commit to GIT `git add . && git commit -m "TUT-2: Rebase commit 1"`
 4. Create a new feature branch `git checkout -b rebase-feature` this will represent our feature branch.
 5. In the feature branch update the `example.txt` file to include `Feature commit 1`. Once that's done add the file and commit `git add . && git commit -m "TUT-2: Feature commit 1"`
 6. Checkout the merge branch, add a new line to the example.txt and commit it. This will represent another commit being added to our main branch. 
`git checkout rebase`
`echo "Rebase commit 2" >> example.txt`
`git add . && git commit -m "TUT-2: Rebase commit 2"`
7. Once again checkout our feature branch, add a new line to the example.txt and commit it. This will represent the fact that we're actively working on this particular feature whilst the main branch is still being worked on.
`git checkout rebase-feature`
`echo "Feature commit 2" >> example.txt`
`git add . && git commit -m "TUT-2: Feature commit 2"`
8. For good measure, lets add another line to the example file and commit that. 
`echo "Feature commit 3" >> example.txt`
`git add . && git commit -m "TUT-2: Feature commit 3"`
9. We'll now rebase in the main branch, `rebase` into the feature branch to represent us rebasing in the latest code from the main branch ready for a pull request.
`git rebase -interactive rebase` 
This command is iterrating through the commits we've done and forcing us to deal with the merge conflicts as a result on each one. It's important to note that the code will represent how it looked at that given point in time. Once each merge conflict has been resolved, simply add the file and continue on the rebasing. 
`git add . && git rebase --continue` 
10. Now let's look at the git history for what we've just done `git log`. You should see that all the feature commits have been pushed onto the end of the main branch but we don't have a new commits to show that we've resolved merge conflicts.
11. Now let's see the commits in a slightly different way `git log --graph --decorate --pretty=oneline --abbrev-commit`

### 3. Squashing commits in a branch via rebasing

 1. Create a new branch `git checkout -b squashing-commits`
 2. Create a new file with some basic content `echo "Hello World" > example.txt`
 3. Commit this file to the git history `git add . && git commit -m "TUT-3: Create example.txt"`
 4. Make 3 more commits to the git history, for example add more content to the example file under three commits maybe add one of the commit messages as `WIP: Some extra content`
 5. Have a look at the commits in the git history `git log`, you should see your 4 commits with the latest at the top. 
 6. Now we're going to squash these commits down into a single more meaningful commit using rebasing `git rebase -i HEAD~4`
 7. This should show you the last 4 commits from HEAD with the oldest at the top and newest at the bottom. When rebasing always start at the bottom and make your way to the top. You'll need to put your terminal editor into edit more, this is `i` in VIM. 
 8. When you're in the edit mode, starting from the bottom replace the word `pick` for `f` or `fixup` for 3 of the commits. This will push the changes from this commit to the previous commit and disregard the commit message. ***Please note:*** You should always leave the first one intact. 
 9. Once this has been done, exit edit mode and save the changes in VIM this is done via `esc` followed `SHIFT + :` then `wq` for write quite and `enter`
 10. You've successfully squash 4 commits into 1. 

### 4. Rebasing a branch into your branch 

 1. Checkout the branch `rebasing-branch` this was created prior to updates to the readme, so should serve the purpose of this nicely. 
 2. Make some changes to the readme file and commit them.
 3. Now checkout the main branch and ensure you've got the latest `git checkout main && git pull origin main`
 4. After you've got the changes for the main branch, checkout the rebasing branch `git checkout rebasing-branch`
 5. We'll rebase in the main branch into our feature branch `git rebase -i main`. ***Please note:*** It's advised to create a backup branch before doing this until you've gain confidence in rebasing. This will ensure that if it goes wrong you've not lost anything and have a fallback. 
 6. You'll have to resolve the merge conflicts and add them via `git add .` followed by `git rebase --continue`. When we're rebasing here, the branch we're rebasing in is considered to be HEAD but if you're unsure you can always abort the rebase with `git rebase --abort`. 
 7. Finally, when you're pushing the commits back up to a remote branch you'll need to do `git push origin <BRANCH> --force-with-lease`.

### 5. Pulling from origin with rebase

 1. `git pull origin <BRANCH> --rebase`

### 6. Merging pull request strategies
  
