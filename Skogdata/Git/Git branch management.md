

[Clean up old git branches | Nicky blogs (nickymeuleman.netlify.app)](https://nickymeuleman.netlify.app/blog/delete-git-branches)

List all local merged branches.

`git branch --merged`

List all local unmerged branches.

`git branch --no-merged`

List all remote merged branches.

`git branch -r --merged`

List all remote unmerged branches.

`git branch -r --no-merged`

`git branch -d <branchname>`
`git branch -D <branchname>`
`-d` will only delete if branch is merged to current branch
`-D` will force delete

`git branch --merged | egrep -v "(^\*|master|dev)" | xargs git branch -d`
`egrep -v "(^\*|master|dev)"` excludes branches named master and dev
`xargs git branch -d`: Deletes every remaining git branch piped in by previous operation

If you wish to completely clean house and delete unmerged branches, change `--merged` to `-no--merged` and change the lowercase `-d` to the uppercase `-D`.

### Deleting non-existent tracking branches

You might have branches locally that have since been deleted remotely.

`git remote prune <remote> --dry-run`

This command will list all branches that were set up to follow remote branches if that remote branch has been deleted. To delete the branches it listed, leave off the `--dry-run`.

In my example project with the single remote named “origin” that becomes

`git remote prune origin`

That deleted the branches `remotes/origin/apollo-frontend` and some others for me. Those were deleted on GitHub, but not locally.

### Deleting a single remote branch

`git push <remote> --delete <branch>`

In my example project I could delete the branch `remotes/origin/lint` with

`git push origin --delete lint`

This will work for both merged and unmerged branches, but only for branches you own!

### Deleting multiple remote branches
`git branch -r --merged | egrep -v "(^\*|master|dev)" | sed 's/origin\///' | xargs -n 1 git push origin --delete`

`sed 's/origin\///'`: The previous command returns strings of the form `“origin/<branch>“`. This filters the “origin/” out.

`xargs -n 1 git push origin --delete`: Deletes every remaining git branch.

[How to Delete a Branch in Git Locally and Remote? - Hatica](https://www.hatica.io/blog/deleting-git-branches/)

[git - Delete all branches that are more than X days/weeks old - Stack Overflow](https://stackoverflow.com/questions/10325599/delete-all-branches-that-are-more-than-x-days-weeks-old)