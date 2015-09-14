# Git

Source control should be part of your day to day tool box. If it is not I recommend that you take the time to learn how to use it. The benefits you will rip will surpass any time you invest in it. [1]

It's relationship with Puppet is in two aspects. One, keeping track of your work and second module distribution.

I recommend that you make sure you know about the following terms and that you feel confident using them.

## Things to know

* Initializing a Repository
* Commit Changes
* Branching
* View Changes
* Merging branches
* Tagging
* Cloning, Pulling and Pushing from remote repositories

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

Being able to easily create, checkout and merge branches in one of Git's major features. And a major asset for us as it allow us to test new features in our Puppet code without risk of breaking our stable code.

Add a `develop` branch to your project and add a `Feature1.txt` file to it.

```bash
vagrant:$ cd ~/git-project
# create a new branch
vagrant:$ git branch develop

# list branches
vagrant:$ git branch
  develop
* master

# checkout the develop branch
vagrant:$ git checkout develop
Switched to branch 'develop'

vagrant:$ git branch
* develop
  master
  
# add and commit a file to develop
vagrant:$ echo 'This is feature #1' > Feature1.txt
vagrant:$ git add Feature1.txt
vagrant:$ git commit -m "Add Feature1"
```

You now have an alternate history in your repository. 

List the contents of your project and then checkout the `master` branch. You will see that the `Feature1.txt` in no longer present in your directory. Since that file was commited to your `develop` branch, it only exists on it.

```bash
vagrant:$ cd ~/git-project
vagrant:$ ls
Feature1.txt  Gemfile  Gemfile.lock

# check our master branch
vagrant:$ git checkout master
vagrant:$ ls
Gemfile  Gemfile.lock

```

## View Changes

### Changes in your branch

Use the `git diff` command to view changes in your current branch.

Update the version of puppet in your `Gemfile` to 3.7.4 and list the differences.

```bash
vagrant:$ cd ~/git-project
vagrant:$ git checkout develop
vagrant:$ sed -i 's/4.2.1/3.7.4/' Gemfile

vagrant:$ git diff
diff --git a/Gemfile b/Gemfile
index 8d31bc1..f0c8937 100644
--- a/Gemfile
+++ b/Gemfile
@@ -2,4 +2,4 @@
 source "https://rubygems.org"

 # gem "rails"
-gem 'puppet', '4.2.1'
+gem 'puppet', '3.7.4'
```

The output reflects what files have been updated and what lines are different from the last commit.



### Between Branches

Use the `git diff REF` command to view differences between branches or commits.

List the differences between our `master` and `develop` branches.

```bash
vagrant:$ cd ~/git-project
vagrant:$ git checkout develop

vagrant:$ git diff master
diff --git a/Feature1.txt b/Feature1.txt
new file mode 100644
index 0000000..66da8e4
--- /dev/null
+++ b/Feature1.txt
@@ -0,0 +1 @@
+This is feature #1
diff --git a/Gemfile b/Gemfile
index 8d31bc1..f0c8937 100644
--- a/Gemfile
+++ b/Gemfile
@@ -2,4 +2,4 @@
 source "https://rubygems.org"

 # gem "rails"
-gem 'puppet', '4.2.1'
+gem 'puppet', '3.7.4'
```

The output of `git diff master` shows us:
* There are changes in the Feature1.txt and the Gemfile files
* Feature1.txt does not exist in the `master` branch
* Gemfile has been updated in `develop`
* Changes in `develop` as marked by a [+] and changes in `master` are marked by a [-]

Commit the changes to Gemfile with a meaningful message.

```bash
vagrant:$ git add Gemfile
vagrant:$ git commit -m "Changed Puppet version to 3.7.4"
```

## Merging Changes

Now that your new feature is ready to go out to production, we need to merge our changes in `develop` back to our `master` branch.

Use the `git merge BRANCH` command to merge the changes from BRANCH into your current branch.

Merge the changes in `develop` into `master`.

```bash
vagrant:$ cd ~/git-project
vagrant:$ git checkout master

# merge your changes back to master
vagrant:$ git merge develop
Updating ff3497b..88468f4
Fast-forward
 Feature1.txt | 1 +
 Gemfile      | 2 +-
 2 files changed, 2 insertions(+), 1 deletion(-)
 create mode 100644 Feature1.txt

# list your directory Feature1.txt will be present
vagrant:$ ls
Feature1.txt  Gemfile  Gemfile.lock

# list your commits in a pretty way ;)
vagrant:$ git log --decorate --oneline --graph
* 88468f4 (HEAD, master, develop) Changed Puppet version to 3.7.4
* aa9ec9e Add Feature1
* ff3497b This is the initial commit
```

The output not reflects the branch merge. 

And now, if your don't need it any more, you can delete your development branch.

### Deleting a branch

Delete a local branch by using the `git branch -d BRANCH` command.

```bash
vagrant:$ cd ~/git-project
vagrant:$ git branch
* master
develop
vagrant:$ git branch -d develop
vagrant:$ git branch
* master
```

---

[1] Pro Git. A must for learning about the tool, this book has been released under a Creative Commons license. And it is available for download at http://git-scm.com/book/en/v2