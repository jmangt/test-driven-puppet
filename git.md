# Git

Source control should be part of your day to day tool box. If it is not I recommend that you take the time to learn how to use it. The benefits you will rip will surpass any time you invest in it. [1]

It's relationship with Puppet is in two aspects. One, keeping track of your work and second module distribution.

I recommend that you make sure you know about the following terms and that you feel confident using them.

## Things to know

* Initializing a Repository
* Commit Changes
* Branching
* Tagging
* Pushing and Pulling from remote repositories
* Re-basing
* Stashing Changes

### Install Git

In an debian based system your can install git via the package manager.

```bash
vagrant:$ sudo apt-get instal git-core
```

### Configure Git

If you have not done so, configure git with your information. Git requires that you provide an email address and a name in order to 'sign' each change to your repository.

Run the following commands to configure git:

```bash
vagrant:$ git config --global user.email "you@example.com"
vagrant:$ git config --global user.name "Your Name"
```

### Initialize a Repository

Start a simple project using Ruby 2.2.1 and Puppet 4.2.1.

```bash
vagrant:$ mkdir git-project

# Initialize RVM project
vagrant:$ echo '2.2.1' > ~/git-project/.ruby-version
vagrant:$ echo 'puppet-4.2.1' > ~/git-project/.ruby-gemset

# Install gem dependencies
vagrant:$ cd ~/git-project
vagrant:$ bundle init
vagrant:$ echo "gem 'puppet', '4.2.1'" >> Gemfile
vagrant:$ bundle install
```

Now you can turn this project into a git repository using the `git init` command.

```bash
vagrant:$ cd ~/git-project
vagrant:$ git init
Initialized empty Git repository in /home/vagrant/git-project/.git/
```

Your directory is now a fully operational git repository. Awesome! (I love git)

### Commit Changes


Check the status of your code by using the `git status` command.

```bash
vagrant:$ git status
On branch master

Initial commit

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	.ruby-gemset
	.ruby-version
	Gemfile
	Gemfile.lock

nothing added to commit but untracked files present (use "git add" to track)
```

Add untracked or updated files to the stage area using the `git add FILES` command.

```bash
vagrant:$ git add .
vagrant:$ git status
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   .ruby-gemset
	new file:   .ruby-version
	new file:   Gemfile
	new file:   Gemfile.lock
```

Our files are ready to be committed to the repository. Use the `git commit -m 'commit message'` command to "save" your staged changes to source control.

```bash
vagrant:$ git commit -m "This is the initial commit"

[master (root-commit) c6c9fe9] This is the initial commit
 4 files changed, 25 insertions(+)
 create mode 100644 .ruby-gemset
 create mode 100644 .ruby-version
 create mode 100644 Gemfile
 create mode 100644 Gemfile.lock
```

You can now check the status and history of your repository by using the `git log` command. The command will show a list of all commits made to the repository and show information about them. 

```bash
vagrant:$ git log

commit c6c9fe911fe4b74e4496ab5db61b1cc9381bc2f8
Author: JManGt <me@jmang.gt>
Date:   Mon Sep 14 01:50:07 2015 +0000

    This is the initial commit
```

# Branching

Up to this point all the work you have done has been done under the default branch of your repository. This branch is called the `master` branch. 

You can think of a branch a just like you would of a tree branch. In this case a branch is the history of commits that split from the main history of the tree's trunk (master branch).

Each branch you create allows you to add changes to your repository without risk of breaking your stable code.

Being able to easily create, checkout and merge branches in one of Git's major features.




---

[1] Pro Git. A must for learning about the tool, this book has been released under a Creative Commons license. And it is available for download at http://git-scm.com/book/en/v2