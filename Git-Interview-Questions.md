# Git Interview Questions

1. **What is Git?**
   - Git is a distributed version control system that tracks changes in source code during software development. It allows multiple developers to work on a project simultaneously.

2. **What is the difference between Git and GitHub?**
   - Git is a version control system, while GitHub is a web-based platform that uses Git for version control. GitHub provides a platform for collaboration and hosting Git repositories.

3. **Explain the basic Git workflow.**
   - The basic Git workflow involves three main stages: working directory, staging area, and repository. Changes are made in the working directory, staged using `git add`, and then committed to the repository with `git commit`.

4. **What is a repository in Git?**
   - A Git repository is a storage location where a project's version-controlled files and history are kept. It can be local (on your machine) or remote (on a server like GitHub).

5. **How does Git handle branching?**
   - Git handles branching by creating separate lines of development. Each branch represents a different version of the code, allowing developers to work on features or bug fixes independently.

6. **What is a Git merge?**
   - Git merge combines changes from different branches into a single branch. It is commonly used to integrate changes from feature branches into the main branch.

7. **Explain the difference between 'git pull' and 'git fetch'.**
   - `git pull` fetches changes from a remote repository and merges them into the current branch. `git fetch` only fetches changes without merging, allowing you to review the changes before merging.

8. **What is a Git commit?**
   - A Git commit is a snapshot of changes made to a repository. It includes a unique identifier (hash), a commit message describing the changes, and references to the previous commit(s).

9. **How do you resolve a merge conflict in Git?**
   - Merge conflicts occur when Git is unable to automatically merge changes. To resolve conflicts, you need to manually edit the conflicting files, mark them as resolved with `git add`, and then complete the merge with `git commit`.

10. **What is Git rebase, and when would you use it?**
    - Git rebase is used to combine or modify a sequence of commits to create a linear history. It can be used to keep the commit history clean by incorporating changes from one branch into another.

11. **Explain the concept of Git tags.**
    - Git tags are references to specific points in Git history, usually used to mark release points. Unlike branches, tags do not change and are typically used for version numbers.

12. **How do you undo the last Git commit?**
    - To undo the last Git commit, you can use `git reset HEAD^` to unstage the changes or `git reset --hard HEAD^` to remove the commit and discard the changes.

13. **What is the purpose of the '.gitignore' file?**
    - The `.gitignore` file specifies intentionally untracked files that Git should ignore. It is used to exclude files or directories from being tracked by version control.

14. **How can you revert a commit that has already been pushed and shared with others?**
    - To revert a commit that has been pushed, you can use `git revert` to create a new commit that undoes the changes made in the previous commit. This approach avoids rewriting history.

15. **What is Git bisect used for?**
    - Git bisect is used for binary search through the commit history to find the commit that introduced a bug. It helps in pinpointing the exact commit where the issue was introduced.

These questions cover a range of Git concepts and operations, and interviewers may tailor them based on the level of the position and the specific requirements of the role.
