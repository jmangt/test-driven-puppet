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

## Installing Git

In an debian based system your can install git via the package manager.

```bash
vagrant:$ sudo apt-get instal git-core
```

## A simple example

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

---

[1] Pro Git. A must for learning about the tool, this book has been released under a Creative Commons license. And it is available for download at http://git-scm.com/book/en/v2