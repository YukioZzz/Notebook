### keeping a fork up to date

#### 1. Clone your fork:

    git clone git@github.com:YOUR-USERNAME/YOUR-FORKED-REPO.git

in particular, use `--depth <depth>` to create a shadow clone

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

### delete file and its history
删除文件命令如下(若是文件夹，加参数`-r`即可)：

    git filter-branch --force --index-filter 'git rm --cached --ignore-unmatch targetFile' --prune-empty --tag-name-filter cat -- --all

### What if there are changes both at local and remote repo
- if have the total permission:
  - git commit
  - git pull --rebase (or git fetch + git merge/rebase [specify_branch])
- otherwise:
  - git stash
  - git pull
  - git stash pop
