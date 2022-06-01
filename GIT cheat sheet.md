# [[GIT]] cheat sheet

## create & clone
**create new** repository
```
git init
```

**clone local** repository
```
git clone /path/to/repository
```

**remote** repository
```
git clone username@host:/path/to/repository
```

## add & remove
**show** file change status
```
git status
```

**show** untracked file change status
```
git status -u
```


**add** changes to INDEX
```
git add <filename>
```

**add all** changes to INDEX
```
git add .
```
**add all** changes to INDEX ещё вариант
```
git add *
```

**remove/delete**
```
git rm <filename>
```

## commit & synchronize
**commit** changes
```
git commit -m "Commit message"
```

**push** changes to remote ropository
```
git push origin master
```
**connect** local repository to remote repository
```
git remote add origin <server>
```

**update** local repository with remote changes
```
git pull
```

## branches
**show** all branches
```
git branch
```

**create** new branch
```
git branch <branch>
```

**rename** branch
```
git branch -M <new_branch_name>
```


**switch** to branch
```
git checkout <branch>
```

**create and switch** to new branch
```
git checkout -b <branch>
```

**delete** branch
```
git branch -d <branch>
```
**push branch** to remote repository
```
git push origin <branch>
```

## merge
**merge changes** from another branch
```
git merge <branch>
```

**view changes** between two branches
```
git diff <source_branch> <target_branch>
```

## tagging
**create tag **
```
git tag <tag> <commit ID>
```

**show commits**
```
git log
```

**show commits** as a graph
```
git log --graph
```
