## How to remove file from repository history

```
git filter-branch --index-filter 'git rm -rf --cached **/*.jar'
```

should work, but it's a bit silly because git globs (`*`) match path separators. So, `**/*.jar` is equivalent to `*.jar`.

This also means that `*/a*.jar` matches `dir1/abc/dir2/log4j.jar`. If you want to match something like `**/a*.jar` (all jars whose name starts with `a` in any directory), you should use find. Here's a command to remove any jars whose names start with `a` or `b`, and any jars in `dir1/dir2`, and any .txt file in any directory:

```
git filter-branch --index-filter 'git rm -rf --cached "*.txt" "dir1/dir2/*.jar" $(find -type f -name "a*.jar" -o -name "b*.jar")'
```

* https://stackoverflow.com/questions/38880436/git-remove-all-of-a-certain-type-of-file-from-the-repository

In other words, the simple case would be:

```
git filter-branch --index-filter 'git rm -rf --cached *.jar'
```

Now doing a`git push` may not work because the local and remote history are different as described here:

>Your use of filter branch has made your repository different to the remote. So as you observe, the remote rejects your push with a warning about this. In this case you are planning to forcibly change the repository to match your filtered version so you can go ahead and push with the force flag. You shouldn't change published repositories like this -- but you can if you want to. Bear in mind anyone who has a clone needs notifying - but if there aren't any others - then go ahead.

If you want to be really careful - make a another clone of the original now. Then do your push. If something ends up wrong - for can always force it back using `push --mirror` from the backup repository:

https://stackoverflow.com/questions/8165099/git-push-after-git-filter-branch-rejected

In that case just force the push to the remote repo:

```
git push -f <remote> <branch>
```

or

```
git push origin master --force-with-lease
```

as described here: https://stackoverflow.com/questions/10510462/force-git-push-to-overwrite-remote-files

Apparently, the second option is safer when other people are contributing to the same repo, but this is not important because I am the only one working on it.

## Miscellaneous Links

* LTspice .ignore file: https://techoverflow.net/2020/06/17/what-gitignore-to-use-for-ltspice-xvii-projects/
* Sample .gitignore file: https://gist.github.com/octocat/9257657
* 

