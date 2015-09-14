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




---

[1] A successful Git branching model: http://nvie.com/posts/a-successful-git-branching-model/
