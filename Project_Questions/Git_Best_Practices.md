## 1. What is Git?

**Git** is a **Version Control System (VCS)**. Think of it like a powerful, digital time machine for your code.

* It tracks every change made to your files.
* It allows you to look back at any previous version, undo mistakes, and coordinate work among multiple people.
* Because it's **distributed**, every user has a complete copy of the project's history on their computer.

***

## 2. What is the difference between merge and rebase?

Both **merge** and **rebase** combine changes from one branch into another, but they do it in different ways:

| Feature | `git merge` | `git rebase` |
| :--- | :--- | :--- |
| **History** | **Keeps** the full, chronological history. | **Rewrites** history to create a straight line. |
| **Commit** | Creates a **new "merge commit."** | Moves your commits to the end of the target branch. **No extra merge commit.** |
| **Commit Graph** | **Messy/Non-linear** (Shows when branches joined). | **Clean/Linear** (Makes history look simple). |
| **Best Used For** | Merging into long-lived branches like `main` or `dev`. | Cleaning up your local feature branch before merging it into `dev`. |

***

## 3. What is a Pull Request (PR)?

A **Pull Request (PR)** (or Merge Request) is not a Git command itself, but a **feature of hosting services** like GitHub.

It's a way to tell others, "I've finished my work on this branch. Please review my changes, run tests, and if everything looks good, **pull** my code into the main branch."

It's the official process for **code review and quality gatekeeping**.

***

## 4. How do you resolve merge conflicts?

A **merge conflict** happens when two different branches change the exact same lines of code (or one person deletes a file that another person modified). Git can't decide which change is correct, so it stops and asks you.

To resolve a conflict:

1.  **Open the file** that Git marks as having a conflict.
2.  Look for conflict markers: `<<<<<<<`, `=======`, and `>>>>>>>`.
3.  **Manually edit the file** to choose the code you want (keeping changes from both sides, or just one).
4.  **Remove** the conflict markers.
5.  **Stage** the edited file: `git add <file-name>`.
6.  **Commit** the merge to finish the process.

***

## 5. What are Git tags?

**Git tags** are like permanent, unmoving labels or stickers you put on a specific commit in your history.

* They are typically used to mark **major release points** of a project, such as **v1.0.0** or **v2.1-beta**.
* Unlike branches, tags **never move**. They permanently mark a known, good version of the code.

***

## 6. What is Git workflow?

A **Git workflow** is a defined set of rules, strategies, and procedures that a team follows to use Git effectively. It dictates:

* **When** to create branches (e.g., for every feature or bug fix).
* **How** to name branches (e.g., `feature/login` or `fix/bug-45`).
* **How** to integrate changes (e.g., always use a Pull Request to merge into `dev`).

The most famous professional strategy is called **Git Flow** (which you partially used: `main`, `dev`, `feature`).

***

## 7. Explain `git stash`

The `git stash` command is used to **temporarily save your uncommitted changes** and clean your working directory.

Imagine you're working on Feature A, but suddenly a critical bug needs fixing *right now*. You haven't finished Feature A yet, but you can't commit incomplete code.

* You run `git stash` to save your changes in a hidden stack.
* Your working directory is now clean, and you can switch to the bug-fix branch.
* Once the bug is fixed, you switch back to Feature A's branch and run `git stash pop` to **re-apply** your saved changes exactly where you left off.

***

## 8. What is the use of `.gitignore`?

The **`.gitignore`** file is a plain text file where you list the file names and patterns that **Git should completely ignore** and *not* track.

It is used to keep the repository clean and small by ignoring files that:

1.  Are **temporary** or created automatically (like log files: `*.log`).
2.  Contain **private or sensitive data** (like configuration files or passwords).
3.  Are **very large binaries** (like compiled code).