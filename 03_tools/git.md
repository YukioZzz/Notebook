### keeping a fork up to date

#### 1. Clone your fork:

    git clone git@github.com:YOUR-USERNAME/YOUR-FORKED-REPO.git

#### 2. Add remote from original repository in your forked repository: 

    cd into/cloned/fork-repo
    git remote add upstream git://github.com/ORIGINAL-DEV-USERNAME/REPO-YOU-FORKED-FROM.git
    git fetch upstream

#### 3. Updating your fork from original repo to keep up with their changes:

    git pull upstream master

### change git remote

    git remote remove origin
    git remote add origin git@github.com:YukioZzz/xxx.git

### merge

merge `branchB` into `branchA`

    git checkout branchA
    git merge branchB

merge commits
    
    git rebase -i <commit_sha> 

### update the final commit

    git commit --amend  

### go back to the history

    git reset --hard commit_id  # where commit_id can be replaced by HEAD^/HEAD^^

### Overwrite local files
    
    git fetch --all
    git reset --hard origin/master

### patch

    git diff CMakeLists.txt > patch01CMakeList
    or
    diff -u hello.c hello_new.c > hello.patch

usage(-p[num] tells the patch command to skip 3 leading slashes from the filenames present in the patch file.):
    
    patch -p[num] < patchfile
