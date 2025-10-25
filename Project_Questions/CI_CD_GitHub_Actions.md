## 📝 Git and Version Control Interview Prep

## 1. What is Git?

**Git** is a **Version Control System (VCS)**. Think of it like a powerful, digital time machine for your code.

* It **tracks every change** made to your files.
* It allows you to look back at any previous version, undo mistakes, and coordinate work among multiple people.
* Because it's **distributed**, every user has a complete copy of the project's history on their computer.

***

## 2. What is the difference between merge and rebase?

Both **merge** and **rebase** combine changes from one branch into another, but they do it differently:

| Feature | `git merge` | `git rebase` |
| :--- | :--- | :--- |
| **History** | **Keeps** the full, chronological history. | **Rewrites** history to create a straight line. |
| **Commit** | Creates a **new "merge commit."** | Moves your commits to the end of the target branch. **No extra merge commit.** |
| **Commit Graph** | **Messy/Non-linear** (Shows when branches joined). | **Clean/Linear** (Makes history look simple). |
| **Best Used For** | Merging into long-lived branches like `main` or `dev`. | Cleaning up your local feature branch before merging it into `dev`. |

***

## 3. What is a Pull Request (PR)?

A **Pull Request (PR)** (or Merge Request) is not a Git command itself, but a **feature of hosting services** like GitHub.

It's a way to tell others, "I've finished my work on this branch. Please review my changes, run tests, and if everything looks good, **pull** my code into the main branch." It is the primary process for **code review and quality gatekeeping**.

***

## 4. How do you resolve merge conflicts?

A **merge conflict** happens when two different branches change the exact same lines of code. Git stops and asks you to manually decide which code is correct.

1.  **Open the file** that Git marks as having a conflict.
2.  Look for conflict markers: `<<<<<<<`, `=======`, and `>>>>>>>`.
3.  **Manually edit the file** to choose the correct code (combining changes or picking one version).
4.  **Remove** the conflict markers.
5.  **Stage** the edited file: `git add <file-name>`.
6.  **Commit** the merge to finish the process.

***

## 5. What are Git tags?

**Git tags** are like **permanent, unmoving labels** you put on a specific commit in your history.

* They are typically used to mark **major release points** of a project, such as **v1.0.0**.
* Unlike branches, tags **never move**. They permanently mark a known, stable version of the code for easy reference.

***

## 6. What is Git workflow?

A **Git workflow** is a defined set of rules, strategies, and procedures that a team follows to use Git effectively. It dictates:

* **When** to create branches (e.g., for every feature or bug fix).
* **How** to name branches (e.g., `feature/login` or `fix/bug-45`).
* **How** to integrate changes (e.g., always use a Pull Request to merge into `dev`).

The strategy we used with `main`, `dev`, and `feature` branches is part of the professional **Git Flow** model.

***

## 7. Explain `git stash`

The `git stash` command is used to **temporarily save your uncommitted changes** and clean your working directory.

It's used when you need to switch branches quickly to work on an urgent bug fix, but you haven't finished your current work yet and can't commit incomplete code. You run `git stash` to save the work, fix the bug, and then run `git stash pop` to **re-apply** the saved changes exactly where you left off.

***

## 8. What is the use of `.gitignore`?

The **`.gitignore`** file is a plain text file where you list the file names and patterns that **Git should completely ignore** and *not* track.

It is essential to keep the repository clean and small by ignoring files that:

1.  Are **temporary** or created automatically (like log files: `*.log`).
2.  Contain **private or sensitive data** (like configuration files or passwords).
3.  Are **very large binaries** or system files that don't need version control.
```