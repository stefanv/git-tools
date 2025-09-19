# Useful Git commands and aliases

## Scripts

Under `scripts` you will find tools that, if added to your path,
provide new git commands.  For example, if `git-pr` is on your path,
you can now use `git pr`.

- `nr`: Add new remote for GitHub/GitLab user X
- `pr`: Fetch one or more pull requests to branch pr/X.
- `rmm`: Remove merged & squash merged branches locally.
         It should be straightforward to also remove remote branches; any takers?
- `brhist`: Show last modification date of each branch
- `wipe`: Permanently delete files from repository by rewriting history.

## Aliases

These go under the `[alias]` section in `~/.gitconfig`.

```
# List branches by date
bbd = "for-each-ref --sort=committerdate refs/heads/"

# Print the current branch name
branch-name = "!git rev-parse --abbrev-ref HEAD"

ci = commit
d = diff --color-words
dd = diff --color-words --cached
st = status
co = checkout
br = branch
cp = cherry-pick
ga = annex

# Pull from the upstream repository
pom = "pull origin main"  # or "pull upstream main", depending how you name remotes
fom = "fetch origin main"  # or "pull upstream main", depending

# Email log; useful for grabbing commit messages for pasting into, e.g., GitHub
el = log --pretty=e

# List all remote branches
rbr = "!f() { git branch -r | grep "^[[:space:]]*$1.*" | cut -d '/' -f 2 | sort | uniq | column ; }; f"

# Print log graph
slog = log --oneline --topo-order --graph

# Print log graph with more info
slog2 = log --graph --topo-order --abbrev-commit

# Rebase on fetched origin/master
rb = "!f() { git fetch origin master && git rebase -i origin/master ;}; f"

# New branch X from fetched origin/master
new = "!f() { git fetch origin master && git checkout -b $1 origin/master ;}; f"

# Set upstream branch to X
upstream = "!f() { git branch --set-upstream-to=$1 $(git branch-name) ;}; f"

# Remove branches that have been merged
delete-merged-branches = "!git co master && git branch --no-color --merged | grep -v '\\*' | xargs -n 1 git branch -d"

# Pull PR nr X to branch prX
pullpr = "!f() { git fetch origin pull/$1/head:pr$1 ;}; f"

# Force push without overwriting someone else's work
fp = "push --force-with-lease"

# git diff, but word-by-word instead of line-by-line
wdiff = diff --color-words
```

## Configuration

I recommend:

```
git config --global merge.conflictstyle zdiff3
```

It makes resolving conflicts so much easier, since it shows you the
common ancestor, thereby helping you figure out the *intent* of
changes.

For example, consider the following diff:

```diff
<<<<<<< HEAD
position = velocity * time + 0.5 * acceleration * time**2
=======
position = velocity * time
>>>>>>> merged-branch
```

All you see is two formulas, and you must choose the right one. 🤔

But, look at the `diff3` version:

```
<<<<<<< HEAD
position = velocity * time + 0.5 * acceleration * time**2
||||||| merged common ancestor
position = velocity * time + acceleration * time
=======
position = velocity * time
>>>>>>> merged-branch
```

Immediately, you see what happened. On `HEAD`, the ancestor commit's
approximation is corrected by introducing the proper 0.5 scaling
factor on the acceleration term. And it is clear that removing this
term (as in `merged-branch`) is not the correct solution.
