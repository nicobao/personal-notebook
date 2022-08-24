## My opinionated Git development workflow

I use a variant of [Trunk-based development](https://platform.deloitte.com.au/articles/semantic-versioning-with-conventional-commits).

## TL;DR

### Main workflow

1. only one public branch: `main`
1. every start of sprint, we agree on tasks to achieve and the order in which they should be merged
1. split each task in issues as small as possible. Issues name should follow [conventional commits](https://www.conventionalcommits.org/en/v1.0.0/)
1. open DRAFT PRs and start working: 
	- Only 1 dev per PR. 
	- Only 1 dev per branch. 
	- Each PR addresses 1 issue only. 
	- If the PR relates to the issue `11`, call the feature branch `fix-context-11` depending on issue name
1. on the feature branch, `git pull --rebase origin main` or `git rebase -i origin/main` then `git push --force-with-lease` every morning to fix merge conflicts
1. dev owning the feature branch keeps pushing every evening
1. reviewers keep reviewing every morning
1. Once PR is ready:
	1. `fixup` all commits except the first commit, `reword` the first commit to the name of the PR, using `git rebase -i origin/main` and edit the commit body to a relately comprehensive and meaningful content. Usage of `conventional commits` is mandatory here. Enforce conventional commits using pre-commit hooks.
	1. change DRAFT PR to normal PR. The CI automatically runs test on this branch.
	1. if the tests pass, reviewers make their last review.
1. Once the PR is reviewed and acceped, the PR is squashed and merged (not rebased) into main. Delete the associated feature-branch.
1. The merge triggers a CI pipeline that will run tests and all sorts of checks, then generate artifacts such binaries or docker images from the code, generate changelog. No tags or GitHub release are generated as the versions are deemed unstable at this point.
1. At the end of the sprint, after UAT has passed and a version is ready for deployment in production, or whenever the maintainers are happy, maintainers are responsible for manually creating a git tag and associated GitHub Release using [semantic-release](https://github.com/semantic-release/semantic-release). This should be done as a manual job in the CI pipeline - and maintainers would click on it if and only if they want to validate a specific commit version to production. The release will contain all the relevant generated Changelog since the previous tag.

### What to do when the production version 1.0.0 requires 1.0.1 but untested ongoing work has already been merged in main?

1. `git checkout 1.0.0`
1. `git checkout -b hotfix-1.0.0`
1. `git push --set-upstream origin hotfix-1.0.0` - this branch is a public branch, NOT a feature branch.
1. Send PRs to branch `hotfix-1.0.0` using `fix` in commit message and relying on the workflow described above
1. Merge PRs to `hotfix-1.0.0`. `hotfix-XXXX` branches should trigger the same CI as `main`, and allow to create tags/releases.
1. Create release/tag `1.0.1` from the relevant git hash in this branch
1. To get the changes to `main` as well:
	1. Send the same PR to `main` (usually the best choice) OR
	1. Create a branch `merge-hotfix-1.0.0` from `hotfix-1.0.0`, run `git checkout merge-hotfix-1.0.0 && git merge main`, resolve conflict, merge `merge-hotfix-1.0.0` into `main`, and delete `merge-hotfix-1.0.0`
1. Do NOT delete the branch `hotfix-1.0.0`.

### What to do when my work depend on a feature branch waiting to be merged (opened DRAFT PR for example)?

If it's a feature branch then it will rebased to main frequently, you can't work on it as if it was a public branch. Do NOT do this:
	1. Create a branch called for example `my-work` from the other feature branch called `base-feature-branch-name`
	1. Open a DRAFT PR that will merge `my-work` into `base-feature-branch-name`
	1. Keep doing the same workflow as explained above, except with `base-feature-branch-name` instead of `main`
	1. Make sure to merge `my-work` into `base-feature-branch-name` BEFORE merging `base-feature-branch-name` into `main`. If you fail doing so, it's no big deal: just rebase `my-work` to `main` and change the PR target branch to `main`.

Instead:
- commit together in the same branch (bad idea), OR:
- simply don't do that. Divide the feature in smaller chunks that is dispatched between different people and independent between each other.

---

### What to do when two development happens simultaneously (experimenting with a complex feature for instance)?

_WARNING!_ I don't recommend doing this at all, it is generally a bad idea. 99% of the time it is unecessary and cumbersome.

1. Create a new branch for the experimental dev
1. Send PRs to that branch and trigger CI as if it was `main`
1. Keep going with the normal workflow for the rest of the development
1. Eventually merge the experimental branch into main


