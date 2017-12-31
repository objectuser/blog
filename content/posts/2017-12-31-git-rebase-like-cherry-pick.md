---
title: "Git Rebase Like Cherry Pick"
date: 2017-12-31T16:00:00-06:00
---

Sometimes I want to move a commit between branches. This is easy with
`git cherry-pick`.

However, sometimes I want this commit inserted into my branch at a
specific point. This can be done with an interactive rebase.

An interactive rebase lists the commits to be applied to the branch with
respect to, usually, the `master` branch. I usually use `git rebase -i`
to squash commits. But there's nothing magic about the list of commits
provided by the rebase. You can remove them, add to them, change them,
whatever.

Let's see an example.

```
git rebase -i master
```

This invokes your configured editor with the list of commits currently
in effect for the branch. For example:

```
pick aaaa1234
pick bbbb1234
pick cccc1234
```

These lines can be edited, deleted, whatever. But new lines can also be
added. If, for example, I have a commit, `dddd1234`, and I want to add
it to the current branch, I can just add it to the list:

```
pick aaaa1234
pick bbbb1234
pick cccc1234
pick dddd1234
```

When I exit the editing session, those commits will be applied. I can do
the same sort of thing to delete the commit from any other branch.

In the above example, it's really no different from a cherry pick.
However, I don't have to apply the commit to the end. I can put it
anywhere I like:

```
pick aaaa1234
pick dddd1234
pick bbbb1234
pick cccc1234
```

That can be useful in some situations, especially if I'm replacing other commits:

```
pick aaaa1234
pick dddd1234
```

Powerful stuff.
