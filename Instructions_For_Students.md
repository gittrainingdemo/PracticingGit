#Instructions for Students#

The following serves as an introduction to the Feature Branch way of working with git and GitHub, along with some practice with merge conflicts and recovering from mistakes.

This lesson works best if you have a friend to do it with, to fully experience resolving merge conflicts.  After you find a friend, figure out who will be "User 1" and who will be "User 2", as you'll have separate instructions later.

###Getting started with these Excercises###
This lesson set assumes you already have a git installation.  [GitHub for Windows](https://windows.github.com/) or [GitHub for Mac](https://mac.github.com/) are good clients if you don't already have one.

Fork this repository (only once, if you have a partner) and then clone it.  Either hit the convenient "Clone in Desktop" button (if you installed a client recently, this may require you to restart your browser), or 
```
git clone https://github.com/[username]/PracticingGit.git
```
Be sure to give your friend [push access](https://help.github.com/articles/adding-collaborators-to-a-personal-repository)!

###My first feature, GUI-style###
Let's get a feel for our new GUI (unless you are a strict command-line person, then skip ahead to the next section).

First, let's make a branch off of master.  Note the new branch will always be off of whatever branch you are currently working on.  Use your name as the branch.

Second, open up the README.md file.  User 1, replace [training aid #1] with your name.  Likewise, User 2, replace [training aid #2] with your name.  We want to avoid conflicts for now, so make sure you don't edit anything else.  Be sure to save your file.

You should see the client recognize the changes.  It should even stage them for you automatically by checking the box of all the files with edits (currently only one).

Type in a commit message, optionally a commit body and then commit

----GUI
- make branch
- make changes to contributor file (avoiding merge conflict) 
- stage
- commit 
- sync

----Command line
- make branch
- make changes (add favorite recipe to bottom of recipes, making conflict)
- stage
- commit
- push
- resolve conflicts


###Staging###
(have them checkout a branch, git reset --soft HEAD~1)


###Log###
-introduce ranges here
-quitting out of less

###Reset/Revert###
-use revert to temporarily rollback changes for debugging
-use rest

###Merging###

####A basic merge####
We have two branches `quiche-1-merge` and `quiche-2-merge`.  They have some additions to our recipe collection, but the quiche recipes don't quite match.  Let's merge these two branches and resolve the conflicts.
```
git checkout master
git branch integrating-quiche
git checkout integrating-quiche
```
First, since we anticipate a grusome merge, let's make a branch `integrating-quiche` to work on.  If we screw up, we can delete the branch and try again.

```
git merge quiche-1-merge 
```

The syntax here says "merge quiche-1-merge onto the current branch".  This merge should be done automatically, as it was derived from master and has no overlapping changes.

```
git merge quiche-2-merge 
```

Here we have some manual issues to resolve.  GitHub for Windows or GitHub for Mac can't handle these, so you have to resolve them on the command line.

 Open the merge tool to handle this (I recommend [Meld](http://meldmerge.org/)), or resolve them manually using your text editor.
```
git mergetool
```

If you are using Meld, remember that the center editor is what will be the merge results, the outside editors are temporary.  Save (Ctrl+S) when you are done resolving all the conflicts, and then exit Meld.  Git will automatically detect when you exit and move on.

When you are done with the mergetool, you'll see something like:
![image](https://cloud.githubusercontent.com/assets/6819944/3714391/26cb0e32-15a5-11e4-96f1-374f913f91db.png)

If you are happy with the results of the merge, `git commit` will conclude the merge process.  If you want to undo the merging, `git merge --abort` will rollback to before you merged in quiche-2-merge. 

####Merging strategies####
In some cases, we can be smart about how we merge, using *merge strategies*.

For all the examples, we will assume we were the authors of quiche-merge-1 and are looking to merge in some changes posed by quiche-merge-2.

```
git checkout quiche-merge-1
git branch merge-strats
git checkout merge-strats
```

Now, if quiche-merge-2 were an old dev branch and we want to basically ignore all changes in that branch, we can use the *ours* strategy. 

`git merge quiche-2-merge -s ours`

This type of merge will *never* have a conflict, but if you look at the end of the results, **all** changes from quiche-2-merge, including the extensive soup recipe, were lost.  The *ours* strategy is typically only useful if you want to keep the history of a branch, but ignore all consequences of said branch.

Let's undo the merge:
```
git reset --hard HEAD~1
```

A more useful merging pattern would have used the default merging strategy, but defaulted to using our version, if there was a conflict.

Thankfully, there is such an option.
```
git merge quiche-2-merge -X ours
```

Again, there will be no conflicts.  This is because any conflicts are resolved using our changes.

Notice in the results from the merge that all three recipes (quesadilla, soup and quiche) are there, with our quiche recipe overwriting the other one.

The subtle difference here is the -s and the -X switch.  the -s uses the *ours* strategy, where the -X uses the *ours* option associated with the default strategy (known as the *recursive* strategy).

Had we wanted to keep the other quiche recipe (i.e. from "their" branch), we simply would have done:

```
git merge quiche-2-merge -X theirs
```

Most of the time, you'll want to use the default recursive strategy with one of a few options like
```
git merge [branch] -X ignore-all-space   //differences in whitespace are ignored
git merge [branch] -X patience		//can produce better diffs for merging, especially if large changes have been made
```


#Maybe a lesson on dealing with deleted files?#


For a lesson on recovering from mistakes, see [this set of excercises](/Recovering_from_mistakes.md).