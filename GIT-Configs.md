## How to synchronize github fork with "original" repository

```
git clone git@github.com:m-narayan/canvas-lms.git canvas-lms
cd canvas-lms
git remote add upstream https://github.com/instructure/canvas-lms.git
git pull upstream stable
#make sure there is no merge conflits and other problems
#run the test suite to make sure that there is no break in the code
#do run the server in production mode and do a sanity test on browser to make sure the main screens are working
git push
```

## How to merge a branch 

```
git checkout develop
git merge stable
#make sure there is no merge conflits and other problems
#run the test suite to make sure that there is no break in the code
#do run the server in production mode and do a sanity test on browser to make sure the main screens are working
git add .
git commit -m "merge upstream origin/stable/YYYY-MM-DD"
```

 

