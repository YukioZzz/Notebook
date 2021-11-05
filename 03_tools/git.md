### keeping a fork up to date

#### 1. Clone your fork:

    git clone git@github.com:YOUR-USERNAME/YOUR-FORKED-REPO.git

#### 2. Add remote from original repository in your forked repository: 

    cd into/cloned/fork-repo
    git remote add upstream git://github.com/ORIGINAL-DEV-USERNAME/REPO-YOU-FORKED-FROM.git
    git fetch upstream

#### 3. Updating your fork from original repo to keep up with their changes:

    git pull upstream master

### merge commits
    
    git rebase -i <commit_sha> 

### update the final commit

    git commit --amend  

### go back to the history

    git reset --hard commit_id  # where commit_id can be replaced by HEAD^/HEAD^^


