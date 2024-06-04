+++
author = "Elia Migliore"
title = "Iterating on reviews"
date = 2024-05-08T10:06:33+02:00
draft = true
+++

More often than before, due to the fact I'm not relying so heavily on the typesystem during the development (sadly in my job I've switched from Scala to Python) I have the need of searching though the codebase.

I work on a pretty big codebase and each time I need to push for certain changes I would like to search in my changed files.

We also have a single feature single commit requirement for our monorepo so often I tend to search over my changes for a certain stuff, like:
`did I forget an assert somewhere in the prod code?` (I do this to inform mypy that a certain key its present in a dictionary. Hey Elia, if you know this why not enforcing it statically by the type itself?)
Because the world its a ugly and certain codebases are so messed up that fixing properly the typesystem its often bigger than the feature itself, but no, I'm not always such a ugly person, more than once I scope creep my development in the trip of improving the codebase.


Anyway, lets come back to the story.
You want to search something on the list of changed files, what I do tipically its the following:
```bash
> git log --oneline
hash_1 bla bla bla
hash_2 bla bla bla
hash_n ....
hash_n-1 merge_commit in main


> git reset --soft hash_n-1
```

Pro tip: on github/gitlab/bitbucket etc etc there is a button to copy the hash of the previous commit, you can click on it
and run `git rev-parse HASH_COPIED~1` to gather the previous hash where to soft reset (aka move your git `HEAD` pointer that will be used by intellij to gather the files where to search the changes
probably with something like: `git diff --name-only`).

And from now on I can search using pycharm only in my pr:
1. command + shift + f
2. click on `scope`
3. select in the drop menù all changed files


If you are a geek guy you can also do a grep, like: `grep -n 'assert' $(git diff --name-only)`

This is pretty useful while iterating on the review, the last problem its how can we apply the changes to the previous commits?
A awesome tool that one of my colleagues suggested me its [git absorb](https://github.com/tummychow/git-absorb),
you can now return to the previous hash by running `git reset --soft hash_1` and rely on the magic of `git absorb`.