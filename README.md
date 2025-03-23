# days_4-5
Notes and exercises for the last two days
# Day 4
HEAD is a pointer.
## More about git checkout
checkout = switching between different versions of a target entity. An entity can be:
- file
- commit
- branch
So, you could `git checkout branch` to switch to a different branch
But you could also look at a historical commit:
1. Use `git log` to get a hash value for the commit.
2. `git checkout hash_value`
This points the HEAD at the specified commit. It does not undo anything.
You are warned that your HEAD is detached. Really.
If you do want to make changes, you can create a new branch at this historical commit and start making changes.
To exit the git log, type :q and press ENTER.

You only need to use the first 7 characters of the hash. You can get these using the command:
`git log --oneline`

To point HEAD back at the top of the branch:
`git checkout master`

## Restore
If you have made some uncommitted changes and you've made a mess:
`git restore file_name`
HOWEVER, use with caution - you cannot get those changes back, because they were never committed.

More accurately, git restore restores the file back to the HEAD - so if it's not pointing at master or the top of the branch, then be alert to that.

So, that means you can restore to any commit in the log. 
`git restore --source HEAD~2 myfile.txt`
restores to the second commit back from the HEAD. In this case, we are assuming that the HEAD points at the most recent commit. So if that is the third commit, this restores the state of the file after the first commit.

Remove an added (staged) file:
`git restore --staged file_name`
(Best to exit and save before doing the restore. Editors often save the changes.)

## Reset
This is the act of removing a commit to `reset` the branch.
`git reset hash_value`
This removes commit in front of the hash_value (commit), but not the changes in the files.
So, for example, if you have committed two files, this reset won't change them, but it will take them out of the repo and put them back into the working directory.

`git reset hash_value --hard`
This removes the commit and also reverts any changes in the files.
**Assume that a hard reset is not recoverable.**

## Revert
Here is a scenario.
You want to go back to an earlier commit, but someone else has branched from master after that commit. That would cause problems, because a hard reset would remove the commit where the other branch starts from.
Creates a new commit that matches the state of a previous commit.
1. Find the commit (hash) that you want to revert to (use git log)
2. `git revert hash_value` creates a new commit that matches the state of the commit you specified.
will (probably) ask you to change the commit message for this new one...

## Day 4 Exercise
The instructions for the exercise were a bit confusing at one specific point. I restarted and recloned three times at the same point. But it turned out I was doing it correctly all the time. The confusion was a lack of clarity about whether branch was the issue, or detached HEAD.
I implemented the following in full.
1. clone example repo.
2. git log --oneline
3. git checkout 5f2f515
4. git switch main (HEAD now at top of main (master))
5. git checkout 461e423 (second commit - two lines of code.
6. git branch new_branch
   git status 
	HEAD detached at 461e423
	nothing to commit, working tree clean
7. delete two lines and save
8. restore: git restore myfile.txt
9. add an extra line: branch text and save
10. git add myfile.txt
11. git commit myfile.txt -m "new branch code"
12. git status
13. git reset 461e423 (removes commit 00c149c)
14. git switch new_branch
15. git add myfile.txt
16. git commit myfile.txt -m "now committed to the branch"
17. git log --oneline
18. git branch
19. git switch main - see all four lines again

# Day 5
## Stashing
Useful if you have not committed files and you want to look at another branch.
`git stash`
To get them back...
`git stash pop` (also loses the stashed changes)
`git stash other`(keeps the stashed changes so you can pop them again - perhaps in a different branch)
DEFAULT - git does not stash any changes to untracked or ignored files.
Note behaviour:
1. Create Master branch. commit one file.
2. Create Other branch. Create a file bbbbb.txt and save. Do not add or commit.
3. `git switch master` Now - bbbb.txt does not disappear - it is brought over to master branch!!
Can be really difficult if you create conflicts between the two files on the different branch.

### git remote
You can create, view and delete connections to other repositories.
`git remote` lists all the connections you have to remote repostories - these are effectively bookmarked urls
`git remote -v lists all connections and their urls
remove a connection to a repository:
`git remote rm repo_name
add a connection to a repo:
`git add repo_ name url`

### git ignore
Use this to specify files or directories that you don't want to track. For example:
- SQL db (already tracked) `*.sql`
- passwords (specific filename - `all_my_passwords.txt`
- API tokens
- directory `directory_name`
Define these in the .gitignore file in the repo root dir.
Note that if you use the gh desktop, there are templates of common filetypes that you can select

### gh desktop set-up
File => Sign In

If you use commit in the desktop, you don't need to add first. That's done automatically for you.
You can add a file on github.com Add File button.
Note the Account button lefts side - shows you are logged in to gh.com
 - settings / sync / Sign in  & Turn on button. 
Then you can use Source Control to commit and more.
Connect to a repo... if private, thus is the equivalent of the exercise using the PAT for the private tab.
In source control in VSCode:
1. click three dots
2. Select Remote
3. Select Add Remote
4. Paste in url (inc PAT before @github.com if private)
Similar sequence to REMOVE a connection.

View branches in a repo on gh.com
### Issues
A place where you can log questions and issues - like a comments thread. Ideally this should includes a proposed solution.
Isssue tab / New Issue button... input.... Submit issue

## Actions
For example, our Hard reset 

## Pull requests
These are not native to github.
Enable you to set up permissions for apprving merges
Bring your changes into the master/main

Add collaborators
Sends an invitation to the collaborator. Who can then push and pull.

Publish branch... means that it goes from local to github so that others can work on the branch.

## Forking
a githib feature. "Create a copy of a repository on guthub for your own gh account. You can then clone locally, make changes, create a connection to the originla repo (not your forked copy) and then create a pull request. Important to create the connection to original so that you can pull any changes to the original repo - these won't go to your own forked repo on gh.
You actually commit to your forked repo and then create the pull request to the originla from there.
When you clone from gh desktop, it will ask you what your purpose is.

## Actions
this is a platform continuous integration and continuous delivery
create workflows that build and test incoming pull requests, for example.
... or deploy merged pull requests to production
Actions are run automatically in response to trigger events.
Actions themselves are in yml files. Like command-line operations.
	https://docs.github.com/en/actions

Go into the repo in gh. Thefork button is upper right.

Final exersise completed
Forked from a Pieian-Data repo.
Cloned my forked repo.
Added a file saying thank you.
Committed and created pull request.
Closed the pull request.

Certificate is at:
https://www.udemy.com/certificate/UC-c25956e0-dc2c-4198-927b-6ced88da01c8/

