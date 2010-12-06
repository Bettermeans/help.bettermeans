---
layout: default
title: Removing sensitive data
description: Dealing with accidentally committed passwords or other sensitive information
categories: popular git_ninjutsu
terminal_pres: true
---

From time to time users accidentally commit data like passwords or keys into a git repo.  While you can use `git rm` to remove the file, it will still be in the repo's history.  Fortunately, git makes it fairly simple to remove the file from the entire repo history.

Change your password
--------------------

This step should be blatantly obvious, but some users still skip it.  If you committed a password, change it!  If you committed a key, generate a new one.  Once the commit has been pushed you should consider the data to be compromised.  Period.

Purge the file from your repo
-----------------------------

Now that the password is changed, you want to remove the file from history and add it to the `.gitignore` to ensure it is not accidentally re-committed.  For our examples, we're going to remove `Rakefile` from the [GitHub gem](http://github.com/defunkt/github-gem) repo.

    tekkub@iSenberg ~/tmp master*
    $ git clone git@github.com:defunkt/github-gem.git
    Initialized empty Git repository in /Users/tekkub/tmp/github-gem/.git/
    remote: Counting objects: 1301, done.
    remote: Compressing objects: 100% (769/769), done.
    remote: Total 1301 (delta 724), reused 910 (delta 522)
    Receiving objects: 100% (1301/1301), 164.39 KiB, done.
    Resolving deltas: 100% (724/724), done.

    tekkub@iSenberg ~/tmp master*
    $ cd github-gem/

    tekkub@iSenberg ~/tmp/github-gem master
    $ git filter-branch --index-filter 'git rm --cached --ignore-unmatch Rakefile' HEAD
    Rewrite 48dc599c80e20527ed902928085e7861e6b3cbe6 (266/266)
    Ref 'refs/heads/master' was rewritten

This command will run the entire history of the master branch and change any commit that involved the file `Rakefile`, and any commits afterwards.  Now that we've erased the file from history, lets ensure that we don't accidentally commit it again.

If you wish to retain tags you must specify `--tag-name-filter "cat"`, but note that *this will overwrite your existing tags*.

    tekkub@iSenberg ~/tmp/github-gem master
    $ echo "Rakefile" >> .gitignore

    tekkub@iSenberg ~/tmp/github-gem master*
    $ git add .gitignore

    tekkub@iSenberg ~/tmp/github-gem master+
    $ git commit -m "Add Rakefile to .gitignore"
    [master 051452f] Add Rakefile to .gitignore
     1 files changed, 1 insertions(+), 0 deletions(-)

This would be a good time to double-check that you've removed everything that you wanted to from the history.  Note that `git filter-branch` only works on one branch at a time, so you may need to perform the cleanup on other branches as well.  This could be problematic if the branch has a complex merge history.  If we're happy with the state of the repo, we need to force-push the changes to overwrite the remote repo.

    tekkub@iSenberg ~/tmp/github-gem master
    $ git push origin master --force
    Counting objects: 1074, done.
    Delta compression using 2 threads.
    Compressing objects: 100% (677/677), done.
    Writing objects: 100% (1058/1058), 148.85 KiB, done.
    Total 1058 (delta 590), reused 602 (delta 378)
    To git@github.com:defunkt/github-gem.git
     + 48dc599...051452f master -> master (forced update)

### Cleanup and reclaiming space

While `git filter-branch` rewrites the history for you, the objects will remain in your local repo until they've been dereferenced and garbage collected.  If you are working in your main repo you might want to force these objects to be purged.

    tekkub@iSenberg ~/tmp/github-gem master
    $ rm -rf .git/refs/original/

    tekkub@iSenberg ~/tmp/github-gem master
    $ git reflog expire --expire=now --all

    tekkub@iSenberg ~/tmp/github-gem master
    $ git gc --prune=now
    Counting objects: 2437, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (1378/1378), done.
    Writing objects: 100% (2437/2437), done.
    Total 2437 (delta 1461), reused 1802 (delta 1048)

    tekkub@iSenberg ~/tmp/github-gem master
    $ git gc --aggressive --prune=now
    Counting objects: 2437, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (2426/2426), done.
    Writing objects: 100% (2437/2437), done.
    Total 2437 (delta 1483), reused 0 (delta 0)

Note that pushing the branch to a new or empty GitHub repo and then making a fresh clone from GitHub will have the same effect.

Dealing with collaborators
--------------------------

You may have collaborators that pulled your tainted branch and created their own branches off of it.  After they fetch your new branch, they will need to use `git rebase` on their own branches to rebase them on top of the new one.  The collab should also ensure that their branch doesn't reintroduce the file, as this will override the `.gitignore` file.  *Make sure your collab uses rebase and not merge,* otherwise he will just reintroduce the file and the entire tainted history... and likely encounter some merge conflicts.

Cached data on GitHub
---------------------

Be warned that force-pushing does not erase commits on the remote repo, it simply introduces new ones and moves the branch pointer to point to them.  If you are worried about users accessing the bad commits directly via SHA1, you will have to delete the repo and recreate it.  If the commits were viewed online the pages may also be cached.  Check for cached pages after you recreate the repo, if you find any open a ticket on [GitHub Support](http://support.github.com) and provide links so staff can purge them from the cache.

Avoiding accidental commits in the future
-----------------------------------------

There are a few simple tricks to avoid committing things you don't want committed.  The first, and simplest, is to use a visual tool like git-gui or [gitx](http://gitx.frim.nl/) to make your commits.  This lets you see exactly what you're committing, and ensure that only the files you want are added to the repo.  If you're working form the command line, avoid the catch-all commands `git add .` and `git commit -a`, instead use `git add filename` and `git rm filename` to individually stage files.  You can also use `git add --interactive` to review each changed file and stage it, or part of it, for commit. If you're working from the command line, you can also use `git diff --cached` to see what changes you have staged for commit.  This is the exact diff that your commit will have as long as you commit without the `-a` flag.

Other reading
-------------

* [git-filter-branch documentation](http://www.kernel.org/pub/software/scm/git/docs/git-filter-branch.html)
* [Git user manual - Cleaning up history](http://www.kernel.org/pub/software/scm/git/docs/user-manual.html#cleaning-up-history)