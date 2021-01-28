# Use cases of using version control software git
## What is git?

## Adding features to your software

## Create branches for develop of the software new version
`Git` supports making changes on the code safely and merge them to the original code properly after verification of the new code. For this use case, we need Branching out, Making changes to development branch and Merge codes between branches.

### Branching off the stable master branch
First, make sure that your local repo is pointed to `master` branch. Then create a new version of this branch by:

```
git checkout -b "branch_name"
```

### Making changes to the new branch
Now you can freely change or add any files to the new version without worrying about the master branch. When you are confident with all changes, you can stage and commit new/modified files to the new branch.

### Merge code to master branch
To merge, we should be on the branch which we want to merge to. Since we want to add code from a new version to master, we firstly switch to master branch and run the merge command (the option `-n0-ff` is used to retain all commit message prior to the merge)

```
git merge –no-ff
```
### Getting back to Git Hub

It is time to let GitHub know all the things by `git push`

We can do some clean-up by delete the un-wanted developed branch

```
git branch -d "branch_name"
```

https://azuredevopslabs.com/labs/azuredevops/github/#:~:text=Task%201%3A%20Creating%20a%20new,new%20branch%20and%20press%20Enter.