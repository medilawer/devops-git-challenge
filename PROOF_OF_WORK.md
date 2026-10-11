# DevOps Git Challenge — Proof of Work

Student name: Dilawar Khalid  
SAP ID: 49085  
Repository: https://github.com/medilawer/devops-git-challenge  
Merged pull request: https://github.com/medilawer/devops-git-challenge/pull/1

## What happened in each task

1. **Global setup and initialization:** I inspected the working directory, Git version, GitHub CLI, authentication, and global Git identity. Git was installed, but the CLI was missing, so it was downloaded to a temporary directory. The active personal account was medilawer. No global name or email was configured. I set the name to Dilawar Khalid and used 49085@students.riphah.edu.pk, an existing commit email that GitHub attributed to medilawer in the account's repository history. The private email API was unavailable with the existing token scope. I created an empty public repository, cloned it into a dedicated directory, selected main, and verified its remote and visibility.

2. **Initial files and commit:** I created README.md with my name and SAP ID, and created the exact initial deploy.sh requested by the activity. I made it executable, checked its syntax, and ran it to print the main environment message. I committed both files using the required message. The first push exposed a stale global credential helper pointing to a missing CLI binary. I configured a working helper locally in this repository, preserved the global helper settings, and successfully pushed main with its upstream.

3. **Stashing and context switching:** I created development from main and changed only the echo line. Before staging anything, I recorded the status and diff, then stashed the tracked edit with a descriptive message. The stash appeared in the list and the working tree became clean. I switched to main and verified the original message, switched back to development, and used git stash pop. The edit returned and the stash list became empty. I committed the development change with the required message.

4. **Actual merge conflict:** I returned to main and created staging from main, then changed the same echo line and committed it. Merging staging into development produced a real content conflict in deploy.sh and returned exit code 1. I captured the status, unmerged filename, and Git's conflict-marked file before editing it. I removed the markers and kept the required development and staging confirmation messages. Syntax validation and script execution passed, no unmerged files remained, and the resolution merge commit had two parents.

5. **Pull request workflow:** I pushed development and staging with upstreams, opened the pull request from development into main using the required title, and described the stash exercise, conflict, resolution, and validation. I merged it using a merge commit and verified the MERGED state. I retained development and staging for assessment. I switched to main, pulled with --ff-only, and verified the resolved script and the PR merge commit's two parents.

6. **Proof and reflections:** I captured the complete logs below after pulling the merged PR and before creating this proof-file commit. This file records the actual command evidence and answers the reflection questions. It is committed separately with the required proof message and pushed to main. Its own commit hash is intentionally not included inside this file.

## Complete history before the proof-file commit

These logs capture the full history immediately before the proof-file commit. Neither command used a commit limit, and no output lines have been truncated. The proof commit will appear in the final terminal log after this file is committed.

### git log --oneline

```text
c603854 Merge pull request #1 from medilawer/development
e1fb0ec Merge staging into development and resolve deployment conflict
86f5728 Update deployment script for development
2106de1 Update deployment script for staging
43b8d68 Initial commit: add deployment script and README
```

### git log --graph --oneline --all

```text
*   c603854 Merge pull request #1 from medilawer/development
|\  
| *   e1fb0ec Merge staging into development and resolve deployment conflict
| |\  
| | * 2106de1 Update deployment script for staging
| |/  
|/|   
| * 86f5728 Update deployment script for development
|/  
* 43b8d68 Initial commit: add deployment script and README
```

## Reflection answers

**Q1: Git and GitHub.** Git tracks versions of files in a local repository on my computer. I can make commits, compare changes, and work on branches without an internet connection. GitHub hosts Git repositories online so people can share and collaborate on the same project. It adds features such as pull requests, reviews, and issue tracking.

**Q2: Working Directory, Staging Area, and Local Repository.** The Working Directory contains the files I can open and edit. The Staging Area holds the changes I have selected for the next commit. The command git add copies the current version of a file's changes into the Staging Area. The command git commit records the staged snapshot in the Local Repository's history. Edits made after git add need to be added again if I want them included in that commit.

**Q3: git stash pop and git stash apply.** Both commands restore changes saved in a stash to the working directory. The command git stash pop removes the stash after a successful application. The command git stash apply restores the changes but retains the stash so I can use it again. A conflicted pop normally retains the stash, so I need to resolve the conflict and later drop the stash when I no longer need it.

**Q4: git fetch and git pull.** The command git fetch downloads new commits and updates remote-tracking information, such as origin/main. It does not integrate those changes into my current branch. The command git pull fetches and then integrates changes according to its options and configuration, such as by merging or rebasing. In this activity, git pull --ff-only origin main updated main only because Git could move it forward without creating a local merge commit.

## Actual captured terminal evidence

The transcript below contains actual commands, outputs, and exit codes recorded during the activity. An empty output with exit code 0 means the command succeeded without printing anything; for status and unmerged-file checks, this confirms there were no matching changes. The merge failure was expected and was followed by a successful resolution. Authentication token lines are redacted, and no credentials are included.

```text

## Environment inspection

$ pwd
/home/dilawer
[exit code: 0]

$ git --version
git version 2.43.0
[exit code: 0]

$ /tmp/gh_2.81.0_linux_amd64/bin/gh --version
gh version 2.81.0 (2025-10-01)
https://github.com/cli/cli/releases/tag/v2.81.0
[exit code: 0]

$ /tmp/gh_2.81.0_linux_amd64/bin/gh auth status
github.com
  ✓ Logged in to github.com account medilawer (keyring)
  - Active account: true
  - Git operations protocol: https
  Token: [redacted]
  - Token scopes: 'gist', 'read:org', 'repo', 'workflow'

  ✓ Logged in to github.com account DiLix96 (keyring)
  - Active account: false
  - Git operations protocol: https
  Token: [redacted]
  - Token scopes: 'gist', 'read:org', 'repo', 'workflow'
[exit code: 0]

$ git config --global --get user.name
[exit code: 1]

$ git config --global --get user.email
[exit code: 1]

$ /tmp/gh_2.81.0_linux_amd64/bin/gh api user --jq '{login: .login, id: .id, name: .name, type: .type}'
{"id":170651491,"login":"medilawer","name":null,"type":"User"}
[exit code: 0]

$ /tmp/gh_2.81.0_linux_amd64/bin/gh api user/emails --jq '[.[] | select(.verified == true) | {email, primary, verified}]'
{"message":"Not Found","documentation_url":"https://docs.github.com/rest/users/emails#list-email-addresses-for-the-authenticated-user","status":"404"}gh: Not Found (HTTP 404)
gh: This API operation needs the "user" scope. To request it, run:  gh auth refresh -h github.com -s user
[exit code: 1]

$ /tmp/gh_2.81.0_linux_amd64/bin/gh repo view medilawer/devops-git-challenge --json name,url,visibility,isEmpty,defaultBranchRef
GraphQL: Could not resolve to a Repository with the name 'medilawer/devops-git-challenge'. (repository)
[exit code: 1]


## Task 1: Identity and repository initialization

$ /tmp/gh_2.81.0_linux_amd64/bin/gh api repos/medilawer/LLM-Security-Project/commits -f author=medilawer -X GET --jq '[.[] | {login: .author.login, name: .commit.author.name, email: .commit.author.email}]'
[{"email":"49085@students.riphah.edu.pk","login":"medilawer","name":"medilawer"},{"email":"49085@students.riphah.edu.pk","login":"medilawer","name":"medilawer"},{"email":"49085@students.riphah.edu.pk","login":"medilawer","name":"medilawer"},{"email":"49085@students.riphah.edu.pk","login":"medilawer","name":"medilawer"}]
[exit code: 0]

$ git config --global user.name 'Dilawar Khalid'
[exit code: 0]

$ git config --global user.email 49085@students.riphah.edu.pk
[exit code: 0]

$ git config --global --get user.name
Dilawar Khalid
[exit code: 0]

$ git config --global --get user.email
49085@students.riphah.edu.pk
[exit code: 0]

$ /tmp/gh_2.81.0_linux_amd64/bin/gh repo create medilawer/devops-git-challenge --public --description 'DevOps Git class activity by Dilawar Khalid (SAP ID 49085)'
https://github.com/medilawer/devops-git-challenge
[exit code: 0]

$ /tmp/gh_2.81.0_linux_amd64/bin/gh repo view medilawer/devops-git-challenge --json url,visibility,isEmpty
{"isEmpty":true,"url":"https://github.com/medilawer/devops-git-challenge","visibility":"PUBLIC"}
[exit code: 0]

$ /tmp/gh_2.81.0_linux_amd64/bin/gh repo clone medilawer/devops-git-challenge /home/dilawer/devops-git-challenge
Cloning into '/home/dilawer/devops-git-challenge'...
warning: You appear to have cloned an empty repository.
[exit code: 0]

$ git symbolic-ref HEAD refs/heads/main
[exit code: 0]

$ git branch --show-current
main
[exit code: 0]

$ git remote -v
origin	https://github.com/medilawer/devops-git-challenge.git (fetch)
origin	https://github.com/medilawer/devops-git-challenge.git (push)
[exit code: 0]


## Task 2: Initial files and commit

$ bash -n deploy.sh
[exit code: 0]

$ bash deploy.sh
Deploying application to main environment
[exit code: 0]

$ git add README.md deploy.sh
[exit code: 0]

$ git commit -m 'Initial commit: add deployment script and README'
[main (root-commit) 43b8d68] Initial commit: add deployment script and README
 2 files changed, 13 insertions(+)
 create mode 100644 README.md
 create mode 100755 deploy.sh
[exit code: 0]

$ git push -u origin main
/tmp/gh_2.102.0_linux_amd64/bin/gh auth git-credential get: 1: /tmp/gh_2.102.0_linux_amd64/bin/gh: not found
fatal: could not read Username for 'https://github.com': terminal prompts disabled
[exit code: 128]


## Recovering the stale credential helper

$ git config --local --add credential.https://github.com.helper ''
[exit code: 0]

$ git config --local --add credential.https://github.com.helper '!/tmp/gh_2.81.0_linux_amd64/bin/gh auth git-credential'
[exit code: 0]

$ git push -u origin main
To https://github.com/medilawer/devops-git-challenge.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
[exit code: 0]


## Task 3: Context switching with stashing

$ git status --porcelain
[exit code: 0]

$ git switch -c development main
Switched to a new branch 'development'
[exit code: 0]

$ git status --short
 M deploy.sh
[exit code: 0]

$ git diff
diff --git a/deploy.sh b/deploy.sh
index ae62cfc..03c2af4 100755
--- a/deploy.sh
+++ b/deploy.sh
@@ -1,3 +1,3 @@
 #!/usr/bin/env bash
 set -euo pipefail
-echo "Deploying application to main environment"
+echo "Deploying application to development environment"
[exit code: 0]

$ git stash push -m 'Save development deployment message while switching to main'
Saved working directory and index state On development: Save development deployment message while switching to main
[exit code: 0]

$ git stash list
stash@{0}: On development: Save development deployment message while switching to main
[exit code: 0]

$ git status --short
[exit code: 0]

$ git switch main
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
[exit code: 0]

$ cat deploy.sh
#!/usr/bin/env bash
set -euo pipefail
echo "Deploying application to main environment"
[exit code: 0]

$ git switch development
Switched to branch 'development'
[exit code: 0]

$ git stash pop
On branch development
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   deploy.sh

no changes added to commit (use "git add" and/or "git commit -a")
Dropped refs/stash@{0} (bdae10edafb5df02cc2fcedfc3a2e497ddd56f34)
[exit code: 0]

$ git status --short
 M deploy.sh
[exit code: 0]

$ git diff
diff --git a/deploy.sh b/deploy.sh
index ae62cfc..03c2af4 100755
--- a/deploy.sh
+++ b/deploy.sh
@@ -1,3 +1,3 @@
 #!/usr/bin/env bash
 set -euo pipefail
-echo "Deploying application to main environment"
+echo "Deploying application to development environment"
[exit code: 0]

$ git stash list
[exit code: 0]

$ git add deploy.sh
[exit code: 0]

$ git commit -m 'Update deployment script for development'
[development 86f5728] Update deployment script for development
 1 file changed, 1 insertion(+), 1 deletion(-)
[exit code: 0]


## Task 4: Real merge conflict before resolution

$ git switch main
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
[exit code: 0]

$ git switch -c staging main
Switched to a new branch 'staging'
[exit code: 0]

$ git add deploy.sh
[exit code: 0]

$ git commit -m 'Update deployment script for staging'
[staging 2106de1] Update deployment script for staging
 1 file changed, 1 insertion(+), 1 deletion(-)
[exit code: 0]

$ git switch development
Switched to branch 'development'
[exit code: 0]

$ git merge staging
Auto-merging deploy.sh
CONFLICT (content): Merge conflict in deploy.sh
Automatic merge failed; fix conflicts and then commit the result.
[exit code: 1]

$ git status
On branch development
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
	both modified:   deploy.sh

no changes added to commit (use "git add" and/or "git commit -a")
[exit code: 0]

$ git diff --name-only --diff-filter=U
deploy.sh
[exit code: 0]

$ cat deploy.sh
#!/usr/bin/env bash
set -euo pipefail
<<<<<<< HEAD
echo "Deploying application to development environment"
=======
echo "Deploying application to staging environment"
>>>>>>> staging
[exit code: 0]


## Task 4: Conflict resolution and validation

$ cat deploy.sh
#!/usr/bin/env bash
set -euo pipefail
echo "Deploying application to development environment"
echo "Staging configuration merged successfully"
[exit code: 0]

$ bash -n deploy.sh
[exit code: 0]

$ git add deploy.sh
[exit code: 0]

$ git commit -m 'Merge staging into development and resolve deployment conflict'
[development e1fb0ec] Merge staging into development and resolve deployment conflict
[exit code: 0]

$ git diff --name-only --diff-filter=U
[exit code: 0]

$ bash deploy.sh
Deploying application to development environment
Staging configuration merged successfully
[exit code: 0]

$ git show -s --format=%P HEAD
86f572813953d0d1d2c43c05887d4ac48c806415 2106de1e973ed20149fef31303de6d6addd81834
[exit code: 0]

$ git push -u origin development
remote: 
remote: Create a pull request for 'development' on GitHub by visiting:        
remote:      https://github.com/medilawer/devops-git-challenge/pull/new/development        
remote: 
To https://github.com/medilawer/devops-git-challenge.git
 * [new branch]      development -> development
branch 'development' set up to track 'origin/development'.
[exit code: 0]

$ git push -u origin staging
remote: 
remote: Create a pull request for 'staging' on GitHub by visiting:        
remote:      https://github.com/medilawer/devops-git-challenge/pull/new/staging        
remote: 
To https://github.com/medilawer/devops-git-challenge.git
 * [new branch]      staging -> staging
branch 'staging' set up to track 'origin/staging'.
[exit code: 0]


## Task 5: Pull request workflow

$ /tmp/gh_2.81.0_linux_amd64/bin/gh pr create --repo medilawer/devops-git-challenge --head development --base main --title 'Merge development workflow into main' --body-file /tmp/devops-pr-body.md
https://github.com/medilawer/devops-git-challenge/pull/1
[exit code: 0]

$ /tmp/gh_2.81.0_linux_amd64/bin/gh pr merge https://github.com/medilawer/devops-git-challenge/pull/1 --merge
[exit code: 0]

$ /tmp/gh_2.81.0_linux_amd64/bin/gh pr view https://github.com/medilawer/devops-git-challenge/pull/1 --json url,state,mergedAt,mergeCommit,baseRefName,headRefName,title
{"baseRefName":"main","headRefName":"development","mergeCommit":{"oid":"c603854122da96060d5383faa1dbdaffdc287425"},"mergedAt":"2026-10-11T10:52:28Z","state":"MERGED","title":"Merge development workflow into main","url":"https://github.com/medilawer/devops-git-challenge/pull/1"}
[exit code: 0]

$ git switch main
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
[exit code: 0]

$ git pull --ff-only origin main
From https://github.com/medilawer/devops-git-challenge
 * branch            main       -> FETCH_HEAD
   43b8d68..c603854  main       -> origin/main
Updating 43b8d68..c603854
Fast-forward
 deploy.sh | 3 ++-
 1 file changed, 2 insertions(+), 1 deletion(-)
[exit code: 0]

$ cat deploy.sh
#!/usr/bin/env bash
set -euo pipefail
echo "Deploying application to development environment"
echo "Staging configuration merged successfully"
[exit code: 0]

$ bash -n deploy.sh
[exit code: 0]

$ bash deploy.sh
Deploying application to development environment
Staging configuration merged successfully
[exit code: 0]

$ git show -s --format=%P HEAD
43b8d68782f9c41485467ed4e3bcd50719d780df e1fb0ec0cc254b1c445cc79915ed5b1e66e2ee9c
[exit code: 0]


## Task 6: Complete history immediately before the proof-file commit

$ git log --oneline
c603854 Merge pull request #1 from medilawer/development
e1fb0ec Merge staging into development and resolve deployment conflict
86f5728 Update deployment script for development
2106de1 Update deployment script for staging
43b8d68 Initial commit: add deployment script and README
[exit code: 0]

$ git log --graph --oneline --all
*   c603854 Merge pull request #1 from medilawer/development
|\  
| *   e1fb0ec Merge staging into development and resolve deployment conflict
| |\  
| | * 2106de1 Update deployment script for staging
| |/  
|/|   
| * 86f5728 Update deployment script for development
|/  
* 43b8d68 Initial commit: add deployment script and README
[exit code: 0]

$ git branch --show-current
main
[exit code: 0]

$ git status --porcelain
[exit code: 0]

```
