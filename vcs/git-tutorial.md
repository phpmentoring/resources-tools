Working with Git (the basics)
-----------------------------

This tutorial covers the basic commands for working with Git at the command line.  GUI tools are available, but are not covered here.  Some links are provided at the end of the article.

##Setting up 

To contribute to an open source project, or otherwise work with a public repository of interest, you must first create a fork.  To do this, find the repository on GitHub or BitBucket (etc.).  If you don't already have a user account on that provider, you'll have to register for one.  When you're all signed up, hit the 'fork' button on the repository page.  If you weren't automatically transported to your account page by the fork action, head over there now and you should see your fork of the project ready and waiting for you.

To begin working with your fork, you'll need to clone it to your computer.  Make a note of the clone url prominently displayed on your fork screen as you're going to need it in a second.  On the command line, enter the following: 

	$ cd /path/to/where/you/want/the/cloned/repo/to/be
	
	$ git clone https://url.to.your.forked/repository
	
	$ cd repoRootDir
	
At this stage, you have successfully cloned your fork to your computer and git has automatically created a pointer (called a 'remote') to your online fork and has named it 'origin'.  Henceforth, you will use the term 'origin' when pushing changes from your local repo to your online fork or pulling changes from your online fork to your local repo.

While you're busy working away on your fork, you'll probably want to keep it in-sync with  the original project repository (the one you forked).  For instance, if you're contributing a bug-fix or new feature to an open source project, you'll want to sync your local repository with the main project before pushing it to your fork and opening a pull request.  This important step gives you an opportunity to resolve any conflicts that your code may introduce and has the added benefit of keeping you in the project maintainer's good books.

To pull in the latest changes from the original project repository, you need to add it as a remote.  Remember above that git automatically added a remote reference to your fork called 'origin' when you cloned it your computer.  We need to manually add the remote to the original project repository, like so: 

	$ git remote add upstream https://url.to.the.main/repository

Here, we're naming the remote reference to the original project 'upstream'.  You can substitute this name for anything you like, upstream is just a widely used term for the original project.

Now, to pull in the latest updates from the original project repository, run: 

	$ git pull upstream master 
	
This pulls in any changes from the original project repository's master branch.  You can now be sure that you have the most up-to-date version of the code base and can begin work.  Of course, you may find that you now have [conflicts to resolve](http://www.kernel.org/pub/software/scm/git/docs/v1.7.3/user-manual.html#resolving-a-merge).

Working with your local git repository 
--------------------------------------

From the command line, navigate to your git project root: 

	$ cd /path/to/local/project/root 


Make sure you have the latest code from original project repo: 

	$ git pull upstream master
	
Issuing the pull command to retrieve changes from a remote is a combination of two commands that can be called separately: fetch and merge.  By running the pull command, you fetch any changes and merge them straight into your repository's branches.  If you prefer only to update a specific local branch with changes from a remote repository, opt instead for fetch and merge:

	$ git fetch remote-name
	
Make sure you are sitting on the branch you want merge the fetched changes into (see below for instructions on how to check which branch you are on and how to switch and create branches) and run:

	$ git merge remote-name/branch-name
	
Fetch retrieves all updates from the remote repository, including any new remote branches (you will see them listed in the output of the fetch command).  Crucially though, git does not automatically apply the changes, instead it waits for you to manually apply them to your current branch with the merge command, as shown above.
	
To check which branch you are currently on, enter:

	$ git branch
	
You will have at least one branch present (master) with an asterix indicating which branch you are currently on:

	  master
	* my-cool-feature
	  a-silly-idea
	  
To see the list of branches with the short hash and message of the last commit for each, enter:

	$ git branch --verbose

To switch to a different branch, enter: 

	$ git checkout branch-name
	
To create a new branch (it is good practice to create a new branch for each new feature/ bug ﬁx), enter:

	$ git branch new_branch_name 

Checkout the new branch to begin development work: 

	$ git checkout new_branch_name
	
It is possible to create a new branch, and check it out, all in one action:

	$ git checkout -b new_branch_name
 
Once you’ve added your new feature/ ﬁxed a bug, check the status of your branch: 

	$ git status 
	
The git status command presents you with the git index.  Much can be said about the index area, but for the purposes of this tutorial I'll stick to the basics.

The index comprises three areas (in the order they appear):

* Changes to be committed
* Changes not staged for commit
* Untracked files

The areas displayed in the index depend on the state of your repository when you run git status:

* files in your repository that are untracked (i.e., included in your .gitignore file - see below for more info), will be listed in the Untracked files area  

* tracked files that you have edited but not yet staged for commit are listed in the Changes not staged for commit area

* files that you have staged for commit are listed in the Changes to be committed area

To stage a file for commit, enter:

	$ git add path/to/file
	
You can stage files one by one in this manner if, for instance, you do not want to commit all of the edited files in one go (you may want to split them into separate commits).  

Once you have added all the files you want to include in your next commit, enter:

	$ git commit -m 'My interesting and informative commit message'
	
If, on the other hand, you want to commit all the edited files in one commit, and don't relish the thought of adding them all individually, you can bypass the staging area altogether by adding -a to the commit command:

	$ git commit -a -m 'My interesting and informative commit message'
	
You can actually group -a and -m into one:

	$ git commit -am 'My interesting and informative commit message'

Bypassing the staging area can save you time, but beware, it is all to easy to commit a file by mistake using this method - for example, a database config file that holds private passwords.  Adding files individually reduces this risk somewhat as it forces you to pay attention to the files you are including in the commit.

Eventually, you'll want to push your commit (or bunch of commits) to origin (your fork).  Before doing this, it might be a good time to pull from upstream once more.  Depending on your workflow, you may want to merge your development branch back into your master branch before pushing.  Workflows are outside the scope of this tutorial, but the flexibility of git makes almost any workflow possible and it is definitely a topic worth looking into.

If at this point you find you have conflicts, you can open each file and resolve the conflict manually.  If you are already familiar with version control, the characters git adds to conflicted files will be relatively easy to decipher.  [See here](http://www.kernel.org/pub/software/scm/git/docs/v1.7.3/user-manual.html#resolving-a-merge) for detailed instructions on how to resolve merge commits.  

Once you have manually resolved conflicts within your files, run 'git add' on each one and then commit to tell git that you have resolved them.

Push commits on your branch to origin (your fork): 

	$ git push origin branch_name 

Once you have successfully pushed your commits to your fork, you can go online and issue a pull request if you want your changes to be considered for inclusion in the original repository.  
 
If the project maintainer requests changes to your pull request before accepting, edit your branch as required and re-trace the steps above to add, commit and push ﬁles back to your fork, then update your pull request. 

##House-keeping 

Delete old (accepted/ merged) local branches: 

	$ git branch -d branch_name 

Delete obsolete branches that are not fully merged: 

	$ git branch -D branch_name 
	
Delete remote branches (branches on Github or Bitbucket):

	$ git push remote-name :branch-name
	
To tell git to ignore a file that is not yet tracked, create a file in your project root named .gitignore and simply add the path to the file, relative to your project root.  Each file path should occupy a new line in the .gitignore file.

To tell git to start ignoring a file that is already being tracked, add the file path to .gitignore but also run the following command to remove the file from git's cache:

	$ git rm --cached path/to/file
		
You can add .gitignore files to individual directories within your repository specifying files to ignore under that particular directory, but you can achieve the same outcome with a single .gitignore file in your project root.




