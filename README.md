# git-patch-and-rebase
[Purpose](## purpose)
[Patch](## Patch)
    * [Create patch with git diff](### Create patch with git diff)
    * [Create patch with git format patch](### Create patch with git format patch)

## Purpose
This tutorial will show you how to create a patch from your recent commits in your repository. And then, it will also show you how to apply this patch to another repository correctly.

Just a reminder, if your want to develop the previous work in the repo, do it in a separate branch!

Now, let's start!
## Patch
### Create patch with git diff
First, let's clone a repo and make some change:
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
$ git checkout -b PATCH
Switched to a new branch 'PATCH'
$ git apply patch
$ git commit -a -m "Patch Apply"
[PATCH 15695e4] Patch Apply
 1 file changed, 1 insertion(+)
```
Now the ```patch``` has been applied to  the branch ```PATCH```, we can use ```git diff``` to check the difference between the branch of ```PATCH``` and ```fix_empty_README.md```, they will be totally same.


### Create patch with git-format-patch

This time, we will create the ```patch file``` with ```git-format-patch```.
```
$ git checkout fix_empty_README.md
Switched to branch 'fix_empty_README.md'
$ echo "One more line" >> README.md
$ cat README.md 
# example-repo
Add a new line in READMEgit checkout -b fix_empty_README.md!
One more line
$ git commit -a -m "one more line"
[fix_empty_README.md 21641f7] one more line
 1 file changed, 1 insertion(+)
$ git format-patch -M master
0001-Add-a-new-line.patch
0002-one-more-line.patch
```
the option ```-M``` shows the branch to compare with, now there is two files for ```patch```, let's check it:
```
$ cat 0001-Add-a-new-line.patch 
From eb93f969ccc476a2a0050e9ee192216cf282da16 Mon Sep 17 00:00:00 2001
From: jinhangwang <jinhangwang001@gmail.com>
Date: Wed, 25 Feb 2015 11:28:56 -0800
Subject: [PATCH 1/2] Add a new line

---
 README.md | 1 +
 1 file changed, 1 insertion(+)

diff --git a/README.md b/README.md
index 869ef75..fe8efa4 100644
--- a/README.md
+++ b/README.md
@@ -1 +1,2 @@
 # example-repo
+Add a new line in READMEgit checkout -b fix_empty_README.md!
-- 
1.9.3 (Apple Git-50)

$ cat 0002-one-more-line.patch 
From 21641f7b1eee992896ed814bcd34a5c72047987f Mon Sep 17 00:00:00 2001
From: jinhangwang <jinhangwang001@gmail.com>
Date: Wed, 25 Feb 2015 11:59:17 -0800
Subject: [PATCH 2/2] one more line

---
 README.md | 1 +
 1 file changed, 1 insertion(+)

diff --git a/README.md b/README.md
index fe8efa4..544f799 100644
--- a/README.md
+++ b/README.md
@@ -1,2 +1,3 @@
 # example-repo
 Add a new line in READMEgit checkout -b fix_empty_README.md!
+One more line
-- 
1.9.3 (Apple Git-50)
```
This time, more information are offered! We can tell the when and who submitted it, etc.

For the patch created by ```git-format-patch```, we have to use ```am``` to apply it.
```
$ git checkout PATCH
Switched to branch 'PATCH'
$ git am 0002-one-more-line.
Applying: one more line
$ git commit -a -m "PATCH-0002 apply"
```
Then we can check README.md to see if the new line has been added:
```
$ cat README.md
# example-repo
Add a new line in READMEgit checkout -b fix_empty_README.md!
One more line
```
Attention, if there are sever commits between master and fix, it will create patch file for every commit.

