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

