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
