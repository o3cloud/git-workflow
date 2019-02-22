# git-workflow
Git workflow for o3cloud develop team

## Tools

#### Hubflow fork

https://github.com/o3cloud/gitflow/tree/release/1.6.0

  git clone https://github.com/o3cloud/gitflow.git
  git checkout release/1.6.0
  sudo ./install.sh

#### Zsh hubflow plugin

https://github.com/robbyrussell/oh-my-zsh/tree/master/plugins/git-hubflow


## Git Hubflow Workflow (Basic):

#### Sync Branch:
git hf update - this will update master and develop and sync remote branches with local ones (be sure not to put commits into develop or master as it will push these up)
git hf push - this will push your commits in your local branch to the matching remote branch
git hf pull - this will pull the remote commits into your local branch (don't use if the remote branch has been rebased - use git pull origin "your-branch" instead)

#### Feature Branch:
gif hf feature start "my-feature" - this will create a feature branch on origin and local will be based off the latest develop branch (make sure to git hf update before or you will get an error if local develop and remote develop have divereged)
git hf feature finish "my-feature" - this will delete the local and remote branches (only do this after a PR has been merged)
git hf feature cancel -f "my-feature" - this will delete the local and remote branches (only do this if the feature branch was created in error)
git hf feature checkout "my-feature" - this will checkout the feature branch

#### Hotfix Branch:
git hf hotfix start "release-version" - this will create a hotfix branch on origin and local will be based off the latest develop branch (make sure to git hf update before or you get an error if local develop and remote devleop have divereged)
git hf hotfix finish "release-version" - this will delete the local and remote branches and merge the commits of the hotfix branch into master and develop branches - it will also create a release tag that matches the release version on master
git hf hotfix cancel -f "release-version" - this will delete the remote and local branch (only do this if the hotfix was created in error)
git checkout hotfix/"release-version" - this will checkout the hotfix branch (make sure to git hf update first)

#### Release Branch:
git hf release start "release-version" - this will create a release branch on origin and local will be based off the latest develop branch (make sure to git hf update before or you get an error if local develop and remote devleop have divereged)
git hf release finish "release-version" - this will delete the local and remote branches and merge the commits of the release branch both into develop and master - it will also create a release tag that matches the release version on master
git hf release cancel -f "release-version" - this will delete the local and remote branch (only do this if the release was created in error)
git checkout release/"release-version" - this will checkout the release branch (make sure to git hf update first)

#### Preparing a PR:
- put the Aha! Ticket # in PR title with a description
- assign to the proper reviewer
- don't squash the commits until after reviewed
- after review - squash the commits

## Git Hubflow Workflow (Adv):

#### Squashing Commits:
- checkout the branch you want to squash
- git merge-base "my-branch" develop (returns merge-base-hash)
- git rebase -i "merge-base-hash"
- change all commit types to "squash" from "pick" in the text file (except first) & save file
- if you get a no-op message in the text file and still have multiple commits then use the command git rebase -i (without the hash)
- fix any merge conflicts
- you should have one commit
- force update your remote branch: git push origin "my-branch" -f

Resolving merge conflicts with the develop branch that are not squashing related (generally on PRs - auto-merge will show as disabled):
- git hf update
- git rebase develop (while in your branch)
- resolve any merge conflicts

##### Rules to remember:
- don't ever git merge branches together manually (should never run command - git merge)
- squash only after review and before merging PR into develop
