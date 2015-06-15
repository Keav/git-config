# Scaffold Git Projects

## Contents

* .gitignore_global - Machine-wide exludes
* .gitignore - Template `.gitignore` for individual project repo's

## Templates taken from

* https://github.com/github/gitignore/blob/master/Global/Windows.gitignore
* https://github.com/github/gitignore

## Instructions

Using a global `.gitignore` for your specific development environment and set of tools minimises confustion and complexity with other developers, who may use different tools.

It also means that your individual project `.gitignore`'s, that are incuded in your repo, can be much more concise and specific.

Copy `.gitignore_global` to `~/` and then run:
```
git config --global core.excludesfile ~/.gitignore_global
```
to make git start using this system-wide `.gitignore`.

### General Advice

To stop tracking an existing, already tracked file, once you have added it to .gitignore, e.g. `error_log`, run
```
git rm --cached *error_log
```

To remove files from all of history, see **Remove Files from History** below.

## Other Useful Git things...

**Pro Tip!*** If you have branches that you have merged and deleted from your remote (e.g. GitHub.com), you can run the following command to quickly clear the local branches too.

**Ensure you are on MASTER branch and it is synced with remote!!**

```
git branch --merged | grep -v "\*" | xargs -n 1 git branch -d
```

To see the test output of the above before commiting to it, replace the last `git branch -d` with `echo`.

##### Other Useful Merge related tidying

You can delete a merged local branch with:
```
git branch -d branchname
```

If it's not merged, use:
```
git branch -D branchname
```

To delete it from the remote in old versions of Git use:
```
git push origin :branchname
```

In more recent versions of Git use:
```
git push --delete origin branchname
```

Once you delete the branch from the remote, you can prune to get rid of remote tracking branches with:
```
git remote prune origin
```

or prune individual remote tracking branches, as the other answer suggests, with:
```
git branch -dr branchname
```

## Remove Files from History

https://help.github.com/articles/remove-sensitive-data/

**As always, make sure you are on the correct branch before running!**

1. To remove `Rakefile`:

```
git filter-branch --force --index-filter 'git rm --cached --ignore-unmatch src/Rakefile' --prune-empty --tag-name-filter cat -- --all
```

2. To remove a **directory**, add `-r`:
```
git filter-branch --force --index-filter 'git rm --cached -r --ignore-unmatch directory_to_delete' --prune-empty --tag-name-filter cat -- --all
```

3. Once all your cleaning is done, push your cleaning:
```
git push origin --force --all
```

4. To also remove from your tagged releases:
```
git push origin --force --tags
```
