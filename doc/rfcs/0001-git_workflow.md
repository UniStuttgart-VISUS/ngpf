# Git workflow
This is a step-by-step guide to setup your local git and github repositories. The `ngpf-dev` and `ngpf` repositories are considered to contain the same code. The `ngpf-dev` repository is a private repository and contains additional documentation, developer comments, chat sequences and other resources. To be able to conveniently use both repositories the local git repository needs some additional configuration. 

## Configuration
### Step 1
Fork the `ngpf-dev` repository to your github account.

### Step 2
Clone your fork to your local machine.
	
	git clone https://github.com/username/ngpf-dev

The default branch of the `ngpf-dev` repository should be `master-dev`. In this step you also want to define the `private` remote to the original repository (also called `upstream`). 

	git remote add private https://github.com/UniStuttgart-VISUS/ngpf-dev.git 

###Step 3
Create remote connection to the public repository.

	git remote add public https://github.com/UniStuttgart-VISUS/ngpf.git 

Add the branch `master` from the public repository.

	git checkout -b master public/master

## Creating pull requests
It is recommended to create a new branch before making any changes to the repository. This helps the `master-dev` branch to stay clean and commits to the `private` repository can easily be merged. 
### Step 1
Before creating a new branch, `master-dev` should be synchronized with the `private`(`upstream`) remote.

	git pull private master-dev
	git checkout -b fix_bug

### Step 2
Make your changes, commit them and push the branch into your fork.

	git push origin fix_bug

### Step 3
Go to your online fork (https://github.com/username/ngpf-dev) and press the *create pull request* button.


## Sync with public repository
Before synchronizing the public repository, the changes you want to push should be accepted in the `ngpf-dev` repository. Do not push ongoing pull requests or completely unrelated changes! As an alternative to the direct pushing method described below, you can also create a pull request.
### Step 1
Make sure you are in the `master` branch. Merge the private `master-dev` branch in the `master` branch, but don't commit the changes.

	git merge master-dev --no-commit --no-ff --allow-unrelated-histories

### Step 2
Now you are able to exclude files and directories from the commit. Make sure the private content of `master-dev` is not included in your commit.

### Step 3
Commit your changes to the `master` branch and push to the `public` repository.

	git commit -am "commit message"
	git push public master
