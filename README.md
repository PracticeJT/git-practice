Practice Instructions
=====================

First, get git setup (https://help.github.com/articles/set-up-git) and setup an SSH key pair (https://help.github.com/articles/generating-ssh-keys).

Then open git bash.  Two big hints: typing `git` at the bash command prompt will give you the list of all git commands.  Typing `git help <command>` will open a web page with help on using a particular command.

## Practice 1: Basic development workflow

In these exercises we will simulate the development process of a federal team called 'Practice' in which part of the team works within JT.  The JT team will be expected to commit their code on a branch within the FedPracticeJT organization's repository and then request the changes be pulled into the FedPractice organization's repository.  This tutorial will guide you through this process and a few more advanced features of git.

*Step 1*: Clone the `git-practice` repository (https://github.com/PracticeAviJT/git-practice) locally using the `git clone <url>` command under C:\UBS\Dev\workspaces.  Hint: The url to use is the 'SSH' one found at https://github.com/PracticeAviJT/git-practice.  Cool Feature: Click the 'copy to clipboard' button to quickly get the url into the clipboard.

*Step 2*: We ALWAYS want to work from a branch so lets create a branch.  We will name it `create-<login>-txt` (e.g. `create-strameav-txt`).  Branch naming is important and indicates what functionality is delivered from the branch.  Branch scope should small with different branches used to work on different features.  To create a branch, `git branch <branchname>`.

*Step 3*: Lets switch to our newly created branch using `git checkout create-<login>-txt`.  Expert tip: `git checkout -b <branchname>` creates the branch and switches to it in one go.

*Step 4*: Create a file.  From within your new git-practice directory create an empty \<login\>.txt file (e.g. strameav.txt) by typing `touch <login>.txt`.

*Step 5*: Make git aware of your file.  If you type `git status` you'll see the file has not yet been added to git.  The output of git status should be enough to figure out how to add the file.

*Step 6*: Commit to your local repository using `git commit`.  Don't forget to add a comment (`git help commit` to get the exact syntax).

*Step 7*: Lets make our file visible in the remote repository!  Use `git push origin <branchname>` to do this.  Follow up: type `git remote -v` to see what `origin` is an alias for.  After this step go check https://github.com/PracticeAviJT/git-practice to see if your branch is there.

*Step 8*: Now lets go to github and get our change pulled into PracticeJT's repository.  Go to https://github.com/PracticeAviJT/git-practice and click the 'Pull Request' button and create a pull request from `FedPracticeJT:create-<login>-txt -> FedPractice:master`.  You should now see your pull request as 'open' under https://github.com/PracticeAvi/git-practice/pulls.

*Step 9*: Ask Avi to accept your pull request.  Hint: he won't!  It's time to make a 'fix' based on his comment and practice the back and forth around a pull request.  Make the change in \<login\>.txt as requested in the comment, then add/commit/push it to your remote branch just like before.  Look at your pull request again on github, the commit is now automatically included!

*Step 10*: After your pull request is accepted its time to do some merging and cleanup.  There are several ways to do this, we'll practice one here.  Lets merge our task branch into the FedPracticeJT master branch (the 'base branch').

To merge from `create-<login>-txt -> master' we need to first switch our workspace back to the master branch.  You always need to be in the target branch of the merge before running a merge.

`git checkout master`   
`git merge create-<login>-txt`   

Now lets delete the task branch:

`git branch -d create-<login>-txt`

And that's it.  We're now ready to work on another task.  Notice with the ease of git branches you could be switching between many task branches within the same workspace easily.

## Practice 2: Merge conflicts and rebase

Lets practice some more advanced concepts.  Before doing this exercise, make sure to type `git help rebase` into your git bash and read all about this feature.

Create two task branches (`modify-<login>-txt-1`, `modify-<login>-txt-2`) and make changes on both to \<login\>.txt (add a different line of text to the file in each), and commit them.  From the first practice you should know how to do this.

Switch back to your master branch, and lets merge the `modify-<login>-txt-1` branch into `master`.  You should know how to do that from the first practice.

Switch to the `modify-<login>-txt-2` branch.  Now lets rebase this branch with master, `git rebase master`.

You'll notice that this operation will fail due to a merge conflict.  Lets open our file and resolve the conflict manually.  The first section (\<\<\<\<\<\<\< HEAD) represents the master branch (HEAD pointer) version.  The second section (\>\>\>\>\>\>\> \<commit text\>) represents your local version.  Choose which one to use and fix the file so it only has that text.

For example this text:

> <<<<<<< HEAD   
> some text 1   
>  =======   
> some text 2   
> \>\>\>\>\>\>\> some text 2   

becomes (assuming I chose my local version):

> some text 2   

Now save the file and run `git add <login>.txt` to indicate you have resolved the merge conflict successfully.

We can now continue our rebase, `git rebase --continue`.

And that's it. Now our branch is rebased off of master and could be merged back into master as a simple 'fast-forward' merge.  It's a good practice to rebase when changes happen on master after your branch was created.  If we keep repositories and branches small, this situation should be rare though.  The best way to avoid merge issues is to never have to merge!

## Practice 3: Update FedPracticeJT:master from FedPractice:master

Sometimes changes will come into FedPractice:master branch from other sources other than JT (e.g. ldn dev team).  These steps will help you update the JT fork to include those changes.

The basic idea is we will use our local workspace to pull changes from FedPractice:master and push them to FedPracticeJT:master.

First we need to setup the FedPractice repo in our remote list.  Typing `git remote -v` shows you the current list.  To add a new one, `git remote add <alias> <url>`.  `origin-fed` is the recommended name, and the url is the SSH url found on the github page for the FedPractice's git-practice repository (found here: https://github.com/PracticeAvi/git-practice).

Once that's done type `git remote -v` again to see the new alias.  Now lets get syncing!  We can use our master branch for this assuming it's in sync with the FedPracticeJT master branch, otherwise a new branch is fine (e.g. `git branch master-sync origin/master`).

First, pull the changes from `origin-fed`'s `master` branch into our local `master` branch using `git pull origin-fed master`.

Now we simply push to the origin master branch, using `git push origin master`.  And we're done!
