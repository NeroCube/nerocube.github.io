# Nero Blog
:octocat: A little blog for put learning note.

## Run Service

```
hugo server -D
```

# Reset Submodules
If you do have a script that checks out a git repo or you have removed submodules this script fails. I also would reorder the cleanup to the end in case of submodule changes.
```
git clean -xfd
git submodule foreach --recursive git clean -xfd
git reset --hard
git submodule foreach --recursive git reset --hard
git submodule update --init --recursive
```


