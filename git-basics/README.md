# Git Basics

This cheatsheet covers basic commands for creating and working with local and remote Git repositories.

## Creating a Repository

How you create your repository depends on whether or not you're starting with an new, empty project or with a pre-existing project.

### Creating a Local Repository

To create a local repository for a new project, run the following command from the project directory:
```nohighlight
$ git init
Initialized empty Git repository in /Users/adalovelace/lc101/new-project/.git/
```

### Cloning a Remote Repository from GitHub

To create a local version of a pre-existing project on GitHub, visit that project's page and copy the **Clone or download** URL.

![Clone or download URL](images/clone-download-url.png)

Then at a terminal:
```nohighlight
$ git clone PROJECT_URL
Cloning into 'existing-project'...
remote: Counting objects: 13, done.
remote: Total 13 (delta 0), reused 0 (delta 0), pack-reused 13
Unpacking objects: 100% (13/13), done.
```

## Check Repository Status

The `status` command of `git` is one of the most commonly-used. It can tell you which branch you are currently working on, changes between your repo and a remote repo, which files are staged for commit, and which changed or new files are not being tracked.

**No outstanding changes, not connected to a remote**
```nohighlight
$ git status
On branch master
nothing to commit, working tree clean
```

**No outstanding changes, connected to a remote**
```nohighlight
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working tree clean
```

**Outstanding changes, connected to a remote**
```nohighlight
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   README.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	.gitignore
	git-basics/
```

## Staging Files for Commit

Git keeps track of changes to files within your project, but only if you tell it to. To commit changes to new or existing files, you must first *stage* those files using the `add` command.

If you have created a new file, but not staged it, `git status` will show the following:
```nohighlight
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

	newfile.py

nothing added to commit but untracked files present (use "git add" to track)
```

To stage the new file for commit:
```nohighlight
$ git add newfile.py
```

You will see an updated status for the file after staging it:
```nohighlight
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   newfile.py

```

If you modify a file that has already been added to the repository, it's status will be:
```nohighlight
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   new.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

Staging changes for such a file uses the same syntax as for a new file: `$ git add new.txt`

If you have lots of changes or new files to add, you can add them all at once using:
```nohighlight
$ git add .
```

> *NOTE:* Before using `git add .` make sure you know what you'll be staging! Run `git status` first. This will save you lots of headaches, and keep you from staging files unintentionally.

## Committing Files

To commit all staged changes to your *local* repository, use git's `commit` command.
```nohighlight
$ git commit -m "Add amazing new feature"
[master de0d4b6] Add amazing new feature
 2 files changed, 2 insertions(+)
```

 The `-m` flag adds a descriptive message, which will help you quickly tell which changes were included in the commit later on. Use concise, descriptive messages! And don't forget to surround your commit message with quotes.

## Working With Remote Repositories

Working with only a local repository is fine for some purposes, but you'll quickly find it better to connect your local repository to a remote repository. Using a remote repository allows you to:
- easily back up your project
- collaborate with other programmers
- work on the same project on different computers
- make your code accessible to others (to show to a potential employer, for instance)

At LaunchCode we'll always use [GitHub](https://github.com/) to store our remote repositories, but there are other applications for hosting remote repositories, including [GitLab](https://about.gitlab.com/) and [Bitbucket](https://bitbucket.org/).

### Creating a Remote Repo at GitHub

When signed in, you can create a new remote repository by clicking on the plus icon near your profile, at the top right.

![New Repository](images/new-repo.png)

Fill out the resulting form and submit to create a new repository. You now need to connect your local repository to your remote. First, copy the remote repository URL from the the screen you're on:

![Copy URL from new repository](images/copy-url-from-new.png)

If you chose to initialize your repository with a `README` or `.gitignore` file, your screen will look slightly different. Copy the project URL using the **Clone or download** button on your project's page.

![Copy URL from new repo w/ files](images/clone-or-dl.png)

> *NOTE:* The project URL is **not** the same as the URL of your project page on GitHub, although they are similar. Do not copy the URL from the address bar, since this URL does not have the required `.git` extension. Always look for the **Clone or download** button to obtain the project URL.

Then, from the terminal, in your project directory:
```nohighlight
$ git remote add origin PROJECT_URL
```

This command can be verbalized as, "Hey, git, add a new remote repository connection named 'origin', which lives on the web at PROJECT_URL". The first portion of this command -- `git remote add` -- must always be the same. The `origin` piece is a name that we've chosen to refer to our remote repository. If you have multiple remotes, or want to choose a different name, you may do so. However, you must use this name when pushing/pulling code to/from the remote repository. `origin` is a widely-used default name when you only have one remote (as will almost always be the case in LaunchCode classes).

### Forking a Repository at GitHub

A scenario that will occur from time-to-time in LaunchCode courses, and which occurs quite a lot for developers in general, is that of wanting to copy another developer's project and modify it. This process is known as "forking a repository" since if you view a project's history as a timeline, copying it effectively creates a "fork" in that history.

To fork another developer's repository, visit the project at GitHub and hit the Fork button:

![Fork button](images/fork.png)

This will create a *copy* of the remote repository under your own GitHub profile. You will have a snapshot of the other developer's repository, taken at the moment you hit the Fork button.

From your own profile page, you will see the forked repository listed alongside your other repositories. To work on the code, clone the repository to your computer using the method at the top of this cheatsheet.

Forked repositories can easily be identified by the reference to the original project under the project name on your profile.

![Forked repository](images/forked-repo.png)
