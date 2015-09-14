# Gitflow

Back in 2010 Vincent Driessen presented a branching methodology named [Gitflow](http://nvie.com/posts/a-successful-git-branching-model/) [1]. The goal of Gitflow is provide a simple way to generate development branches and be able to merge them back to master in an orderly fashion.

I will strongly recommend that you read the original post 'A successful Git branching model' (http://nvie.com/posts/a-successful-git-branching-model/) where Vincent goes over the methodology in detail.

Gitflow is important to us because it allows us to generate new feature branches without the fear of breaking our release branch. We can then test new features and only release a new version when they are stable.

When we work with Puppet modules we will generate new releases using this work flow. As each module will have a version, Gitflow is the perfect match to generate them. Also it will allow us to work under feature branches and have experimental version of our module's dependencies.

## Workflow

Here is a run down of the Gitflow work flow.

### Basics

* You start with two long lived branches `master` and `develop`
* Production code comes from your `master` branch
* New releases come from your `develop` branch
 
### Master Rules

* It is forbidden to commit any changes directly to `master`
* The only way to update `master` is via a merge from a `release` or `hotfix` branch

### Develop Rules

* It is forbiden to commit any changes directly to `develop`
* The only way to update `develop` is via a merge from a `feature` branch

### Features

* Coding only happens in `feature` branches.
* `feature` branches are split from `develop`
* Once the feature is complete the `feature` branch is merged into `develop` and the `feature` branch is deleted.


### Releases

* `release` branches are split from `develop`
* They act as a way to freeze the code in preparation to a new version being released.
* Bug fixes and light coding is allowed in `release` branches.
* When the release is complete:
  * the `release` branch is merged into master
  * a new `tag` is added to `master` with the version being released
  * the `release` branch is merged back into develop
  * the `release` is deleted

### Hotfixes

* `hotfix` branches are split from `master`
* They are a short lived branches used to fix typos or critical easy to fix bugs that might have escaped our validation of the `release` branch.
* It is forbidden to add new features to a `hotfix` branch.
* When the fix is ready and tested:
  * the `hotfix` branch is merge back into master
  * the `hotfix` branch is merged into develop
  * the `hotfix` is deleted

## Example

This is how git flow would look in the life cycle of a project.

Create and version control a new project.

```bash
vagrant:$ mkdir ~/gitflow-project
vagrant:$ cd ~/gitflow-project
vagrant:$ git init
vagrant:$ echo '# My project' > README.md
vagrant:$ echo '0.0.0' > VERSION
vagrant:$ git add README.md VERSION
vagrant:$ git commit -m "Initial commit"
```

Add and checkout a new `develop` branch.

```bash
vagrant:$ git branch develop
vagrant:$ git checkout develop
```

Add your first feature by creating and checking out a new `feature` branch.

```bash
vagrant:$ git branch feature/my-feature
vagrant:$ git checkout feature/my-feature
```

Code and commit your new feature.

```bash
vagrant:$ echo '# My first feature' > Feature1.md
vagrant:$ git add Feature1.md
vagrant:$ git commit -m "Feature 1 is complete"
```

Merge back your changes into `develop`

```bash
vagrant:$ git checkout develop
vagrant:$ git merge feature/my-feature
```

Delete the `feature/my-feature` branch

```bash
vagrant:$ git branch -D feature/my-feature
```

Prepare for your first release

```bash
vagrant:$ git branch release/0.1.0
vagrant:$ git checkout release/0.1.0
```

Bump your module's version to match the new release.

```bash
vagrant:$ echo '0.1.0' > VERSION
vagrnat:$ git add VERSION
vagrant:$ git commit -m "bump version 0.1.0"
```

Fix any bugs in your release

```bash
vagrant:$ echo 'Die bug die!!!' >> Feature1.md
vagrant:$ git add Feature1.md
vagrant:$ git commit -m "Bug fixed in Feature1"
```

Close and tag your release

```bash
# merge to master
vagrant:$ git checkout master
vagrant:$ git merge release/0.1.0

# tag your release
vagrant:$ git tag -a 0.1.0 -m 'Release 0.1.0'

# merge to develop
vagrant:$ git checkout develop
vagrant:$ git merge release/0.1.0

# remove the release branch
vagrant:$ git branch -D release/0.1.0
```

## Gitflow Plugin

Same yourself some time and typos and install nvie's [git-flow plugin](https://github.com/nvie/gitflow). [2]

The plugin is "*A collection of Git extensions to provide high-level repository operations for Vincent Driessen's branching model*". In essence, the plugin takes care of the repetitive part of the methodology. It also enforces some common sense rules so you don't shoot yourself in the foot.

Read the[ plugin's wiki](https://github.com/nvie/gitflow/wiki/Installation) for instructions on how to install it in your system. [3]

For an ubuntu system install it using the default package manager:

```bash
$ sudo apt-get install git-flow -y
```

A simplified example of the plugin will look like this.

Create and version control a new project.
```bash
vagrant:$ mkdir ~/gitflow-plugin-project
vagrant:$ cd ~/gitflow-plugin-project
vagrant:$ git init
vagrant:$ echo '# My project' > README.md
vagrant:$ echo '0.0.0' > VERSION
vagrant:$ git add README.md VERSION
vagrant:$ git commit -m "Initial commit"
```

Initialize git-flow in your project.
```bash
# Initialize with all defaults
vagrant:(master)$ git flow init -d
Using default branch names.

Which branch should be used for bringing forth production releases?
   - master
Branch name for production releases: [master]
Branch name for "next release" development: [develop]

How to name your supporting branch prefixes?
Feature branches? [feature/]
Release branches? [release/]
Hotfix branches? [hotfix/]
Support branches? [support/]
Version tag prefix? []
```

Start a feature branch

```bash
vagrant:(develop)$ git flow feature start my-feature
Switched to a new branch 'feature/my-feature'

Summary of actions:
- A new branch 'feature/my-feature' was created, based on 'develop'
- You are now on branch 'feature/my-feature'

Now, start committing on your feature. When done, use:

     git flow feature finish my-feature
```

Code and commit your new feature.

```bash
vagrant:(feature/my-feature)$ echo '# My first feature' > Feature1.md
vagrant:(feature/my-feature)$ git add Feature1.md
vagrant:(feature/my-feature)$ git commit -m "Completed Feature 1"
```

Close your feature branch

```bash
vagrant:(feature/my-feature)$ git flow feature finish my-feature
Switched to branch 'develop'
Updating edb1801..8c83b0b
Fast-forward
 Feature1.md | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 Feature1.md
Deleted branch feature/my-feature (was 8c83b0b).

Summary of actions:
- The feature branch 'feature/my-feature' was merged into 'develop'
- Feature branch 'feature/my-feature' has been locally deleted
- You are now on branch 'develop'
```

Start a release

```bash
vagrant:(develop)$ git flow release start 0.1.0
Switched to a new branch 'release/0.1.0'

Summary of actions:
- A new branch 'release/0.1.0' was created, based on 'develop'
- You are now on branch 'release/0.1.0'

Follow-up actions:
- Bump the version number now!
- Start committing last-minute fixes in preparing your release
- When done, run:

     git flow release finish '0.1.0'
```

Bump version and fix bugs

```bash
vagrant:(release/0.1.0)$ echo '0.1.0' > VERSION
vagrnat:(release/0.1.0)$ git add VERSION
vagrant:(release/0.1.0)$ git commit -m "bump version 0.1.0"
vagrant:(release/0.1.0)$ echo 'Die bug die!!!' >> Feature1.md
vagrant:(release/0.1.0)$ git add Feature1.md
vagrant:(release/0.1.0)$ git commit -m "Bug fixed in Feature1"
```

Finish your release
```bash
vagrant:$ git flow release finish 0.1.0 -m "Release 0.1.0"
Switched to branch 'master'
Merge made by the 'recursive' strategy.
 Feature1.md | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 Feature1.md
Switched to branch 'develop'
Already up-to-date!
Merge made by the 'recursive' strategy.
Deleted branch release/0.1.0 (was 8c83b0b).

Summary of actions:
- Release branch 'release/0.1.0' has been merged into 'master'
- The release was tagged '0.1.0'
- Release tag '0.1.0' has been back-merged into 'develop'
- Release branch 'release/0.1.0' has been locally deleted
- You are now on branch 'develop'
```

---

[1] A successful Git branching model: http://nvie.com/posts/a-successful-git-branching-model/

[2] GitFlow plugin: https://github.com/nvie/gitflow

[3] Installing git-flow: https://github.com/nvie/gitflow/wiki/Installation