# git-patch-and-rebase

This tutorial will show you how to create a patch from your recent commits in your repository. And then, it will also show you how to apply this patch to another repository correctly.

Just a reminder, if your want to develop the previous work in the repo, do it in a separate branch!
Then, let's start!


Now, let's clone a repo and make some change:
```
$ git clone https://github.com/jinhangwang/example-repo.git
$ git checkout -b fix_empty_README.md
Switched to a new branch 'fix_empty_README.md'
$ echo Add a new line in README!!! >> README.md
$ git diff
diff --git a/README.md b/README.md
index 869ef75..fe8efa4 100644
--- a/README.md
+++ b/README.md
@@ -1 +1,2 @@
 # example-repo
+Add a new line in READMEgit checkout -b fix_empty_README.md!
```
The result of ```git diff``` is a typical output of ```Diff Patch```,
we can use this output as a patch directly:
```
$ git commit -a -m "Add a new line"
[fix_empty_README.md eb93f96] Add a new line
 1 file changed, 1 insertion(+)
$ git diff master > patch
$ git checkout master
Switched to branch 'master'
Your branch is up-to-date with 'origin/master'.
$ ls
README.md patch
```
Now we are at the branch ```master``` and get a file named ```patch```, which contains the diff information. Now we'll use ```git apply``` to ultilize this patch. In fact, we hardly create a patch at the branch and apply it in another branch (you can simply ```merge``` it). Now we assume the branch ```fix_empty_README.md``` doesn't exist. Normally, we are supposed to create a branch to handler the branches which commit new patches.
```
$ 
$ 





























