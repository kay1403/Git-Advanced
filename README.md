 
````markdown
# Git-Advanced Exercises

This repository contains solutions for the Git Advanced exercises.  
All commands and explanations are documented here for each challenge.
## Part 1: Refining Git History


### Challenge 1: Missing File Fix

**Problem:**  
We forgot to add `test4.md` in the last commit.

**Terminal Commands and Output:**


# Clone the repository and navigate into it
git clone https://github.com/kay1403/Git-Advanced.git
cd Git-Advanced

# Create dummy files
touch test{1..4}.md

# Stage and commit the first file
git add test1.md
git commit -m "chore: Create initial file"

# Stage and commit the second file
git add test2.md
git commit -m "chore: Create another file"

# Stage and commit third and fourth files (forgot test4.md initially)
git add test3.md
git add test4.md
git commit -m "chore: Create third and fourth files"

# Push changes to remote
git push origin main

# Stage missing file and amend the last commit
git add test4.md
git commit --amend -m "chore: Create third and fourth files"

# Create README.md to track Git exercises
touch README.md
git add README.md
git commit -m "docs: Add README to track Git exercises"

# andle rejected push due to remote updates
git pull origin main --rebase
git push origin main

# output

[main b55912c] chore: Create third and fourth files
 Date: Mon Sep 8 15:32:30 2025 +0200
 2 files changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 test3.md
 create mode 100644 test4.md

[main 3d8c468] docs: Add README to track Git exercises
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 README.md


# Explanation:

git add test4.md stages the missing file.

git commit --amend modifies the previous commit to include the missing file.

git push might be rejected if the remote has updates; in that case, use git pull --rebase before pushing again.

README.md was added to track all exercises moving forward.


### Challenge 2: Editing Commit Message

Problem:
The commit message "chore: Create another file" should be "chore: Create second file" for clarity.

Terminal Commands and Output:

# Start interactive rebase to edit the last 2 commits
git rebase -i HEAD~2

# If there is an editor error, set a new editor
git config --global core.editor "nano"

# Re-run interactive rebase
git rebase -i HEAD~2

# Continue rebase after resolving empty commits or conflicts
git rebase --continue

# Force push to update remote after changing history
git push origin main --force


# Output:

[detached HEAD 6999360] docs: Add README to track Git exercises
 Date: Mon Sep 8 15:52:31 2025 +0200
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 README.md
Successfully rebased and updated refs/heads/main.

Enumerating objects: 8, done.
Counting objects: 100% (8/8), done.
Delta compression using up to 4 threads
Compressing objects: 100% (7/7), done.
Writing objects: 100% (2/2), 276 bytes | 276.00 KiB/s, done.
Total 7 (delta 2), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (2/2), done.
 + 75e80f8...f0528bf main -> main (forced update)


# Explanation:

git rebase -i HEAD~2 allows editing the last 2 commits.

If the default editor fails (vi), switch to nano using git config --global core.editor "nano".

git rebase --continue applies remaining commits after conflicts or skipped cherry-picks.

git push --force updates the remote branch after rewriting history.


### Challenge 3: Squashing Commits

**Problem:**  
We have multiple commits for small changes:
- `chore: Create initial file`
- `chore: Create second file`
- `chore: Create third file`
- `chore: Create fourth file`
- `docs: Add README to track Git exercises`

We want to **squash some commits** to make the Git history cleaner.

**Terminal Commands and Output:**


# Start interactive rebase for the last 2 commits (or more if needed)
git rebase -i HEAD~2

# If editor error occurs, set nano as default editor
git config --global core.editor "nano"

# Re-run interactive rebase
git rebase -i HEAD~2

# Continue rebase after resolving skipped commits
git rebase --continue

# Force push to update remote after rewriting history
git push origin main --force


Output:
# Output:

[detached HEAD 75e80f8] docs: Add README to track Git exercises
 Date: Mon Sep 8 15:52:31 2025 +0200
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 README.md
Successfully rebased and updated refs/heads/main.

Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Delta compression using up to 4 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 272 bytes | 272.00 KiB/s, done.
Total 2 (delta 1), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/kay1403/Git-Advanced.git
 + 09db80c...75e80f8 main -> main (forced update)


Explanation:
# Explanation:

git rebase -i HEAD~2 lets you squash commits interactively.

If your default editor fails, use nano with git config --global core.editor "nano".

git rebase --continue applies the remaining commits after resolving skipped commits or conflicts.

git push --force is necessary because squashing rewrites history on the main branch.


### Challenge 4: Splitting a Commit

**Problem:**  
The commit `"chore: Create third and fourth files"` contains two files (`test3.md` and `test4.md`).  
We want to **split it into two separate commits** for better tracking:  
- `"chore: Create third file"`  
- `"chore: Create fourth file"`

**Terminal Commands and Output:**


# Reset the last commit to unstage files
git reset HEAD~1

# Stage and commit the third file separately
git add test3.md
git commit -m "chore: Create third file"

# Stage and commit the fourth file separately
git add test4.md
git commit -m "chore: Create fourth file"

# Stage README.md and commit changes
git add README.md
git commit -m "docs: Add README to track Git exercises"

# View commit history graphically
git log --oneline --graph --decorate

# Push changes to remote repository (force needed due to history rewrite)
git push origin main --force

# Output:

* f0528bf (HEAD -> main) docs: Add README to track Git exercises
* 5774843 chore: Create fourth file
* a33a524 chore: Create third file
* ecd1d04 chore: Create another file
* 27e720b chore: Create initial file

# Explanation:

git reset HEAD~1 unstages the previous combined commit.

Each file is staged and committed separately to have clearer, atomic commits.

git push --force is required because splitting a commit rewrites the history of the main branch.

### Challenge 5: Advanced Squashing

**Goal:**  
Combine the last two commits (`Create third file` and `Create fourth file`) into a single commit named `Create third and fourth files`.

**Terminal Commands and Output:**

```
# Start interactive rebase including the last two commits
git rebase -i HEAD~2

# In the interactive editor, mark the second commit for squashing:
# pick <hash> chore: Create third file
# squash <hash> chore: Create fourth file

# Edit the commit message to:
# chore: Create third and fourth files

# Save and exit the editor. The two commits are now combined.

# Push the changes to remote (force push is required since history was rewritten)
git push origin main --force

Result:

# View the last 5 commits
git log --oneline -n 5


Output:

<new-hash> chore: Create third and fourth files
<hash> docs: Add README to track Git exercises
<hash> chore: Create another file
<hash> chore: Create initial file

# Explanation:

Use git rebase -i HEAD~2 to start an interactive rebase for the last two commits.

Mark the second commit as squash to combine it with the first.

Edit the commit message to reflect the combined change.

git push --force is necessary because rewriting history changes commit hashes on the remote.

Squashing is useful to clean up commit history and combine related changes into a single commit.


### Challenge 6: Dropping a Commit

**Goal:**  
Completely remove an unwanted commit from the Git history.

**Terminal Commands and Output:**

```
# Create a new file and commit it
touch unwanted.txt
git add unwanted.txt
git commit -m "Unwanted commit"

# Save any pending changes before interactive rebase
git add .
git commit -m "temp: save README changes before rebase"

# Start an interactive rebase including the last 3 commits
git rebase -i HEAD~3

# In the interactive editor, locate the unwanted commit and replace 'pick' with 'drop'

# Continue the rebase
git rebase --continue

# Verify the commit has been removed
git log --oneline

# Output:

# The "Unwanted commit" no longer appears in the history
<hash> docs: Add README to track Git exercises
<hash> chore: Create third and fourth files
<hash> chore: Create another file
<hash> chore: Create initial file

# Explanation:

Always commit or stash changes before performing an interactive rebase to prevent conflicts or errors.

git rebase -i HEAD~3 allows editing the last three commits, including the unwanted one.

Replace pick with drop to remove the unwanted commit.

git rebase --continue applies the remaining commits after dropping the unwanted one.

This ensures your Git history is clean and unwanted commits are fully removed.

git push --force is required because splitting a commit rewrites the history of the main branch.


### Challenge 7: Reordering Commits

**Objective:**  
Delve deeper into `git rebase -i`. Rearrange commits within your history using interactive rebasing.

**Terminal Commands and Output:**

```
# Check the commit history to identify commits to reorder
git log --oneline -n 5

# Output:

0dbe910 temp: save README changes before rebase
74b8057 chore: Create third and fourth file
ecd1d04 chore: Create another file
27e720b chore: Create initial file

# Start an interactive rebase on the last 3 commits
git rebase -i HEAD~3
In the interactive editor, reorder commits by moving the lines corresponding to the commits.

For example, to move chore: Create third and fourth file after temp: save README changes:

pick 0dbe910 temp: save README changes before rebase
pick 898c88f temp: save pending changes
pick 74b8057 chore: Create third and fourth file

# Resolve conflicts if any
git status
git add <conflicted_file>
git rebase --continue

# Finalize the rebase and confirm the new commit order
git log --oneline --graph --decorate

# Final Output:

4ee916c chore: Create third and fourth files
0dbe910 temp: save README changes before rebase
ecd1d04 chore: Create another file
27e720b chore: Create initial file

# Explanation:

Commits have been successfully reordered without losing any changes.

git rebase -i HEAD~3 lets you interactively reorder commits.

Always resolve conflicts carefully with git add <file> and git rebase --continue.

Reordering commits improves clarity and maintains a logical progression of changes.

### Challenge 8: Cherry-Picking Commits

**Objective:**  
Selectively apply commits from one branch to another using `git cherry-pick`.

**Terminal Commands and Output:**

```
# Create a new branch and switch to it
git checkout -b ft/branch

# Add a new file and commit it
touch test5.md
echo "Contenu du test 5" > test5.md
git add test5.md
git commit -m "Implemented test 5"

# Switch back to main branch
git checkout main

# Identify the commit hash from ft/branch
git log ft/branch --oneline -n 1

# Output:

a1b2c3d Implemented test 5

#Cherry-pick the commit into main
git cherry-pick a1b2c3d

# Verify the cherry-picked commit
git log --oneline -n 5

# Output:
a1b2c3d Implemented test 5
<previous commits>

Notes:
If conflicts occur during cherry-pick like in my case:
# Resolve conflicts
git add README.mb
git cherry-pick --continue
Cherry-picking is useful to apply specific commits from another branch without merging the entire branch.


# Challenge 9: Visualizing Commit History

This challenge demonstrates how to visualize Git commit history using terminal commands.

---

## 1. Simple log

```
git log --oneline
Shows recent commits in a single line.

# output:

4ee916c chore: Create third and fourth files
70f32ee temp: save pending changes
c69f446 temp: save README changes before rebase
ecd1d04 chore: Create another file
27e720b chore: Create initial file


2. Log with ASCII graph

git log --oneline --graph
Adds a tree structure to show branch and merge history.

output:

* 4ee916c (HEAD -> main) chore: Create third and fourth files
* 70f32ee temp: save pending changes
* c69f446 temp: save README changes before rebase
* ecd1d04 chore: Create another file
* 27e720b chore: Create initial file


3. All branches with decoration

git log --oneline --graph --all --decorate
Shows all branches (--all) and branch/tags labels (--decorate).

output:

* 84337a2 (ft/branch) Temp: save README changes before switching branch
* 636886d Implemented test 5
* 1d4649a chore: Create third and fourth files
| * 4ee916c (HEAD -> main) chore: Create third and fourth files
| * 70f32ee temp: save pending changes
| * c69f446 temp: save README changes before rebase
|/  
| * 74b8057 (origin/main, origin/HEAD) chore: Create third and fourth file
|/  
* ecd1d04 chore: Create another file
* 27e720b chore: Create initial file

4. Compact formatted log

git log --graph --pretty=format:'%h - %an: %s' --abbrev-commit

%h : abbreviated commit hash

%an: author name

%s : commit message

# output:

* 4ee916c - Ange Koumba: chore: Create third and fourth files
* 70f32ee - Ange Koumba: temp: save pending changes
* c69f446 - Ange Koumba: temp: save README changes before rebase
* ecd1d04 - Ange Koumba: chore: Create another file
* 27e720b - Ange Koumba: chore: Create initial file

These commands give a clear view of the commit history, merges, and branch structure directly from the terminal.

### Challenge 10: Understanding Reflogs (Bonus)

**Objective:**  
Learn how to use `git reflog` to track HEAD history and revert to previous states of your repository.

**Terminal Commands and Output:**

```
# Display the reflog
git reflog

# Output:

4ee916c (HEAD -> main) HEAD@{0}: checkout: moving from ft/branch to main
84337a2 (ft/branch) HEAD@{1}: commit: Temp: save README changes before switching branch
636886d HEAD@{2}: commit: Implemented test 5
1d4649a HEAD@{3}: checkout: moving from 1d4649a9c2fadf3252067e67810835e0eaf2c1a9 to ft/branch
1d4649a HEAD@{4}: rebase (continue): chore: Create third and fourth files
ecd1d04 HEAD@{5}: rebase (start): checkout HEAD~3
4ee916c (HEAD -> main) HEAD@{6}: reset: moving to HEAD
4ee916c (HEAD -> main) HEAD@{7}: rebase (finish): returning to refs/heads/main
4ee916c (HEAD -> main) HEAD@{8}: rebase (continue): chore: Create third and fourth files
70f32ee HEAD@{9}: rebase (pick): temp: save pending changes
c69f446 HEAD@{10}: rebase (continue): temp: save README changes before rebase
ecd1d04 HEAD@{11}: rebase (start): checkout HEAD~3
898c88f HEAD@{12}: rebase (finish): returning to refs/heads/main
898c88f HEAD@{13}: rebase (start): checkout HEAD~2
898c88f HEAD@{14}: rebase (finish): returning to refs/heads/main
898c88f HEAD@{15}: rebase (start): checkout HEAD~2
898c88f HEAD@{16}: commit: temp: save pending changes
0dbe910 HEAD@{17}: rebase (finish): returning to refs/heads/main
0dbe910 HEAD@{18}: rebase (pick): temp: save README changes before rebase
74b8057 (origin/main, origin/HEAD) HEAD@{19}: rebase (start): checkout HEAD~3
e6eedf3 HEAD@{20}: commit: temp: save README changes before rebase
da60e7a HEAD@{21}: commit: Unwanted commit
74b8057 (origin/main, origin/HEAD) HEAD@{22}: rebase (finish): returning to refs/heads/main
74b8057 (origin/main, origin/HEAD) HEAD@{23}: rebase (squash): chore: Create third and fourth file
a33a524 HEAD@{24}: rebase (start): checkout a33a524^
c30078e HEAD@{25}: rebase (finish): returning to refs/heads/main
c30078e HEAD@{26}: rebase (squash): chore: Create fourth file

# Explanation:

HEAD@{0} represents the current state of the repository.

Older entries show previous commits and operations, including checkouts, commits, and resets.

git reflog is extremely useful to recover lost commits, undo mistakes, or return to a previous state even if those commits are no longer visible in git log.