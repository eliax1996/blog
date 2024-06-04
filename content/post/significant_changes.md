+++
author = "Elia Migliore"
title = "How to Use Git Diff to Filter Files with Significant Changes"
date = "2024-06-04"
tags = [
    "git",
]
+++

When working with Git, it's important to be able to track changes and manage them effectively. One useful command for this is git diff, which can be used to compare different commits and identify the files with significant changes.

To filter files with changes greater than 2 lines (added or deleted) in a specific commit, you can use the following command:

`git diff --numstat <first_commit_id>..<second_commit_id> | awk '$2>2 || $1>2' | awk '{print $3}'`

Explanation of the command:

```
    git diff --numstat: This command is used to display the changes between two commits in a numerical format.
    <commit_id>^..<commit_id>: This specifies the range of commits to compare. In this case, the same commit ID is used to compare the changes within the same commit (could also be a relative pointer, like HEAD and HEAD~1).
    awk '$2>2 || $1>2': This awk command filters the output to only show files where either the number of lines added ($2) or deleted ($1) is greater than 2.
    awk '{print $3}': This awk command prints the file names of the filtered files.
```
In the provided context, the command was used to identify files with significant changes due to a large number of file movements.
By filtering out only the necessary files, it becomes easier to cherry-pick these changes into a new branch or revert the large-scale move.

This saved me from an error, moving a gozilion files in another folder while later realizing wasn't great. Now I'm copy pasting only the significant changes in a new branch and I still have the missing files in another branch, when I will decide the right folder I will change the diff before re-applying it in my final branch.