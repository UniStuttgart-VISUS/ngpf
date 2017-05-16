# Git workflow
This is a step-by-step guide to setup your local git and github repositories. The `ngpf-dev` and `ngpf` repositories are considered to contain the same code. The `ngpf-dev` repository is a private repository and contains additional documentation, developer comments, chat sequences and other resources. To be able to conveniently use both repositories the local git repository needs some additional configuration. The naming of the particular branches are just suggestions and can be adjusted to fit your personal pleasure.

## Configuration
### Step 1
Fork the `ngpf-dev` and `ngpf` repositories to your github account.

### Step 2
Clone your fork to your local machine.
	
	git clone https://github.com/<username>/ngpf-dev

The default branch of the `ngpf-dev` repository should be `master-dev`. In this step you also want to define the `private_upstream` remote to the original repository. 

	git remote add private_upstream https://github.com/UniStuttgart-VISUS/ngpf-dev.git 

###Step 3
Create remote connections to the fork of the public

	git remote add public https://github.com/<username>/ngpf.git 

and to the original repositories.

	git remote add public_upstream https://github.com/UniStuttgart-VISUS/ngpf.git 

Add the branch `master` from the public repository.

	git checkout -b master public/master

##<a name="pull"></a> Creating pull requests
The access to the default branches is restricted, this means it is not allowed to push directly. Your code can only propagate into the repositories via pull requests. It is recommended to create a new branch before making any changes. This helps the `master-dev` branch to stay clean and commits/merges to the `private_upstream` repository can easily be merged. 
### Step 1
Before creating a new branch, `master-dev` should be synchronized with the `private_upstream`) remote.

	git pull private master-dev
	git checkout -b fix_bug

### Step 2
Make your changes, commit them and push the branch into your fork.

	git push origin fix_bug

### Step 3
Go to your online fork (`https://github.com/<username>/ngpf-dev`) and press the *create pull request* button.

## Sync with public repository
Before synchronizing the public repository, the changes you want to push should be accepted in the `ngpf-dev` repository. Do not push ongoing pull requests or completely unrelated changes! Then, you can sync the public with the private repository by a pull request.

Make sure you are in the `master` branch and your fork is at the same state as `public_upstream`. Create a new branch.

	git checkout -b public_merge

Merge the private `master-dev` branch in the `public_merge` branch, but don't commit the changes.

	git merge master-dev --no-commit --no-ff --allow-unrelated-histories

Now you are able to exclude files and directories from the commit. Make sure the private content of `master-dev` is not included in your commit. Create a pull request to the private repository as explained [above](#pull).
