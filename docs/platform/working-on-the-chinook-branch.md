# Working on the chinook branch
## Intro
This is a quick howto for working on the 'Charming Chinook' branch. Working on the branch
is easy as we maintain all changes through gerrit.automotivelinux.org. 
If you are unfamiliar with gerrit, please read these fine how-to pages were put together from the
Mediawiki community here: https://www.mediawiki.org/wiki/Gerrit/Tutorial . This covers the basics very well. Of course we'll work with gerrit.automotivelinux.org instead so apply likewise.

## Installation of tools
Install `git` with your distributions package manager.
A very useful tool is "git-review" (cmdline is then `git review`). 
Install it from your distro or with `sudo pip install git-review`.

## Prerequisites
It is important to setup git and gerrit properly (see the Tutorial page mentioned above):

* prereq #1)   make sure git is properly setup with name/email
* prereq #2)   make sure your ssh key is in gerrit.automotivelinux.org

## Cloning, editing and submitting for review

Follow these steps to submit a change to the 'Charming Chinook' branch:

1) cloning the (tip of the) branch
   ```
   repo init -b chinook -m default.xml -u https://gerrit.automotivelinux.org/gerrit/AGL/AGL-repo
   repo sync
   ```

2) Change the recipe in question (one change at a time - small is better)
    ```
    vi recipe-foo/bar/baz.bb
    ```

3) Do a a few builds and tests (try 2 architectures if possible)
   ```
   source meta-agl/scripts/aglsetup.sh .... agl-all-features
   bitbake agl-demo-platform
   ```

4) once satisfied do commit your change as usual in git
   Make sure to do a proper commit message:
   http://chris.beams.io/posts/git-commit/
   ```
   git commit -s
   <enter proper commit message>
   ```

5) All repos have .gitreview files already, so now it is just
   ```
   git review
   ```
   
6) (optional, but highly recommended!) Reset to gerrit/chinook
   ```
   git checkout chinook`
   git reset --hard gerrit/chinook
   ```

   It helps during the review process as gerrit would otherwise enforce
   the whole series of patches to be reviewed/merged together (2nd depends on 1st patch).

7) Rinse (=6) and repeat (=2-5)

## Using git review to review/test-build a specific change in gerrit
'git review' is also useful if you want to review a change.
Example for meta-agl:
```
 repo init -b chinook -m default.xml -u https://gerrit.automotivelinux.org/gerrit/AGL/AGL-repo
 repo sync 
 cd meta-agl/
 git review -d 8105 
```
This will pull-down change 8105. Now we can build with it applied:
```
 cd ..
 source ...
 bitbake ...
````

## Using gerrit to amend a changeset while in review
The same workflow applies if you want to _amend_ a changeset while it is in review (not merged, yet):
```
cd meta-agl/
git review -d 8105
```
This will pull-down change 8105. You can now edit a file:
```
vi recipes-foo/bar/baz.bb
git commit -s --amend 
```

 Don't forget a test build
```
cd ..
source meta-agl/scripts/aglsetup.sh ..... agl-all-features
bitbake ... # e.g. agl-demo-platform
```

 Finally call git review to upload the change
```
git review
```

