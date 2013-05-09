## How to synchronize github fork with "original" repository

```
git clone git@github.com:m-narayan/canvas-lms.git canvas-lms
cd canvas-lms
git remote add upstream https://github.com/instructure/canvas-lms.git
git pull upstream stable
git push
```

## How to merge a branch 

```
git checkout develop
git merge stable
```

 

