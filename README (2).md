# Git Lab Exam -- Command Cheat Sheet

Use **Git Bash**. Run each program in its own project folder unless your
examiner asks you to continue in the same repository.

------------------------------------------------------------------------

## Program 1 -- Create a repository

**What to do:** Create the project folder, initialize Git, create
`README.md`, configure your name/email for this repository, and make the
first commit.

``` bash
mkdir campus-portal
cd campus-portal
git init
git branch -M main
echo "# Campus Portal Project" > README.md
git add README.md
git commit -m "chore: initial project setup"
git config user.name "YOUR NAME"
git config user.email "YOUR_UNIVERSITY_EMAIL"
git status
git log -1
```

**Meaning:** - `mkdir` -- creates the project folder; `cd` -- enters
it. - `git init` -- initializes a Git repository. - `git branch -M main`
-- names the initial branch `main`. - `echo ... > README.md` -- creates
the README file with the project title. - `git add` -- stages the file;
`git commit` -- saves a snapshot. - `git config user.name/user.email` --
sets the author identity for this repository. Ideally configure these
**before** making the commit; if you do it afterward, the earlier
commit's author does not change. - `git status` -- checks whether the
working tree is clean; `git log -1` -- shows the latest commit and its
author.

Expected commit message: `chore: initial project setup`

------------------------------------------------------------------------

## Program 2 -- Create a login feature branch

**What to do:** Start from the repository's `main` branch, create
`feature/login-page`, create `login.html`, then commit it on that
branch.

``` bash
git switch main
git switch -c feature/login-page
notepad login.html
```

Paste this complete HTML into `login.html`, save it, and close Notepad:

``` html
<!DOCTYPE html>
<html>
<head>
    <title>Login Page</title>
</head>
<body>
    <h1>Login Page</h1>

    <label for="username">Username:</label>
    <input type="text" id="username" name="username" placeholder="Enter username">
</body>
</html>
```

Then stage and commit:

``` bash
git add login.html
git commit -m "feat(auth): add login form input"
git branch
git status
git log --oneline --graph --all
```

**Meaning:** - `git switch main` -- moves to `main`. -
`git switch -c feature/login-page` -- creates and switches to the
feature branch. - `notepad login.html` -- opens/creates the HTML file in
Notepad. - `git add` -- stages the file; `git commit` -- records it. -
`git branch` -- lists local branches; `git status` -- checks the current
branch and working tree. - `git log --oneline --graph --all` -- shows a
compact graph of all branches.

**HTML meaning:** - `<!DOCTYPE html>` -- declares an HTML5 document. -
`<html>` -- contains the whole page. - `<head>` and `<title>` -- hold
page information and the browser-tab title. - `<body>` -- contains
visible page content. - `<h1>` -- displays the heading. - `<label>` --
displays the Username label. - `<input type="text">` -- creates a text
box for entering a username.

Commit type: `feat` means a new feature; `(auth)` is the scope, meaning
the authentication/login area.

------------------------------------------------------------------------

## Program 3 -- Create and resolve a merge conflict

**What to do:** On `main`, create and commit `config.json`. Create
`fix/dark-mode` and change the theme to `dark`. Return to `main` and
change the theme to `solarized`. Merge the branches and resolve the
conflict.

First, create the file on `main`:

``` bash
git switch main
notepad config.json
```

Paste and save:

``` json
{
  "theme": "light",
  "version": "1.0.0"
}
```

Commit the initial config:

``` bash
git add config.json
git commit -m "chore: add config file"
```

Create the dark-mode branch and change the theme:

``` bash
git switch -c fix/dark-mode
notepad config.json
```

Change `"theme": "light"` to `"theme": "dark"`, save, then:

``` bash
git add config.json
git commit -m "feat: enable dark mode"
```

Return to `main` and make a different change:

``` bash
git switch main
notepad config.json
```

Change `"theme": "light"` to `"theme": "solarized"`, save, then:

``` bash
git add config.json
git commit -m "feat: enable solarized mode"
```

Attempt the merge:

``` bash
git merge fix/dark-mode
```

A conflict is expected because both branches changed the same line. Open
the file:

``` bash
notepad config.json
```

Git will show conflict markers similar to:

``` text
<<<<<<< HEAD
  "theme": "solarized",
=======
  "theme": "dark",
>>>>>>> fix/dark-mode
```

Remove **all** conflict-marker lines and choose one valid final theme,
for example:

``` json
{
  "theme": "dark",
  "version": "1.0.0"
}
```

Save the file and complete the merge:

``` bash
git add config.json
git commit -m "merge: resolve dark mode conflict"
git status
cat config.json
git log --oneline --graph --all
```

**Meaning:** - `git switch -c fix/dark-mode` -- creates and switches to
the fix branch. - `git merge fix/dark-mode` -- merges that branch into
the currently checked-out branch (`main`). - `<<<<<<<`, `=======`,
`>>>>>>>` -- mark the two conflicting versions; remove them when
resolving. - `git add config.json` -- marks the conflict as resolved;
`git commit` -- records the merge resolution. - `cat config.json` --
displays the final file. It should contain valid JSON and no conflict
markers.

Commit types: `chore` is setup/maintenance; `feat` is a new feature;
`merge` describes the merge-resolution commit.

------------------------------------------------------------------------

## Program 4 -- Push to GitHub and create a Pull Request

**What to do:** This program needs the repository and the
`feature/login-page` branch from Program 2. If starting separately,
first complete the required setup and login branch from Programs 1 and
2. Program 3 is not required.

Add your GitHub repository as the remote (replace the URL with your
own):

``` bash
git remote add origin https://github.com/YOUR-USERNAME/YOUR-REPOSITORY.git
git remote -v
git push -u origin feature/login-page
```

**Meaning:** - `git remote add origin ...` -- connects the local
repository to GitHub. - `git remote -v` -- displays the remote URL. -
`git push -u origin feature/login-page` -- uploads the feature branch
and sets its upstream tracking branch.

### GitHub website steps (as written in the lab question)

1.  Open your repository on GitHub.com.
2.  Create an Issue titled **Implement Login Page Validation**.
3.  Create a Pull Request with **base: `main`** and **compare:
    `feature/login-page`**.
4.  In the PR description, add `Closes #ISSUE_NUMBER` using the actual
    Issue number GitHub assigned.
5.  Add a review comment.
6.  Merge the Pull Request after checks pass.
7.  Confirm that the linked Issue closes automatically.

### Optional: Do the Issue and Pull Request using GitHub CLI

Install GitHub CLI (`gh`) and sign in first with `gh auth login`. Then
use:

``` bash
gh issue create --title "Implement Login Page Validation" --body "Implement login page validation."
gh pr create --base main --head feature/login-page --title "Implement Login Page" --body "Closes #ISSUE_NUMBER"
gh pr comment PR_NUMBER --body "Reviewed the changes. Looks good."
gh pr merge PR_NUMBER --merge
```

Replace `ISSUE_NUMBER` and `PR_NUMBER` with the real numbers. The
Issue-closing keyword must be in the PR description, and the PR must be
merged into the repository's default branch (`main`) for GitHub to close
the Issue automatically.

After merging, update your local `main` and inspect the graph:

``` bash
git switch main
git pull origin main
git log --oneline --graph --all
```

**Meaning:** - `gh issue create` -- creates a GitHub Issue. -
`gh pr create` -- creates a Pull Request. - `gh pr comment` -- adds a
comment to the PR. - `gh pr merge` -- merges the PR. -
`git pull origin main` -- downloads and integrates the latest remote
`main` into local `main`.

------------------------------------------------------------------------

## Quick dependency reminder

-   **Program 1:** foundation; creates the repository.
-   **Program 2:** needs Program 1's repository.
-   **Program 3:** needs Program 1's repository; it does **not** need
    Program 2.
-   **Program 4:** needs the repository and the `feature/login-page`
    branch from Program 2; it does **not** need Program 3.

**Exam tip:** Run `git status` after your work. For the relevant
question, show the verification commands listed in that program. Make
sure the branch names match the question (`main`, `feature/login-page`,
and `fix/dark-mode`).
