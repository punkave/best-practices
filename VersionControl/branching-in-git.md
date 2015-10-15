# Branching in git

Whenever we are working on a new feature, we should create a branch in git. This is straightforward and allows us to commit, push and collaborate before that feature is safe to merge into master or another more mainstream branch.

Here's how to create a new branch with your current changes, push it, and have it ready to go for simply typing "git push" in the future.

Let's say I'm working on the sliding modals feature for A2:

```
git checkout -b sliding-modals
git add -A .
git commit -m "new work on sliding modals"
git push --set-upstream origin sliding-modals
```

Now my current work is in the `sliding-modals` branch.

If I want to go back to working in the `unstable` branch, I type:

```
git checkout unstable
```

## Creating a branch after the fact

Did you wait too long to read this article? Did you push a bunch of half-broken new feature code to the `unstable` or `master` branch? No worries. Here's how to fix it.

*This technique must be used with care. If it was a tiny change, just push a commit that reverses it instead.*

First, create a branch exactly as above:

```
# Make the new branch
git checkout -b sliding-modals
git push --set-upstream origin sliding-modals
```

Now your work is safe in the `sliding-modals` branch. We can safely reset the `unstable` branch to an earlier commit.

Use `git log` to find the *last commit that was not part of your new feature work.* use `git log -p` if you need to see source code changes to figure it out.

Then type:

```
git checkout unstable
# don't do this without everyone's contributions!
git pull
git log [review the results, find the commit you want to go back to]
git reset --hard xxxxxyourcommitidherexxxxxx
git push origin unstable -f
```

**Use the above commands with care. Make  It is possible to lose work permanently if you use `git reset --hard` incorrectly. Make absolutely sure your wew work has already been pushed to your separate `sliding-modals` branch at this point, and that you have everyone else's contributions via `git pull`.**

## Merging a branch

Your work is done and you're ready to reincorporate `sliding-modals` back into the `unstable` branch. Here's how to do that:

```
# make sure you have everyone's contributions
git pull
git checkout unstable
git merge sliding-modals
```

You may be required to resolve some merge conflicts, much as you might after typing `git pull`.
