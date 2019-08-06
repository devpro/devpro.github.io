# Git cheat sheet

[Home](../readme.md) > [git](./git.md) > [cheat sheet](./cheatsheet.md)

## Commands

```bash
# know your reporistory origin URL
git config --get remote.origin.url
git remote -v

# update your origin URL
git remote set-url origin ssh://my.git.url

# reset to previous commit if already pushed
git reset --hard HEAD~1
git push origin HEAD --force

# reset to previous commit if not pushed
git reset HEAD~1

# list all local and remote branches
git branch -a

# fetch data from remotes
git fetch origin

# delete locally dev branch (on wrong remotes for instance)
git branch -d dev

# switch remotes (if branch has been deleted first)
git branch dev -t origin/dev
git checkout dev

# switch upstream on an existing branch
git branch --set-upstream-to=origin/dev dev

# push a branch (master) on a specific remote (other)
git push -u other master

# Navigate through the tags and branches
git log --graph --decorate --oneline --all
```
