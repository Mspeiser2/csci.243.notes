Git is a ___version control system___.

Git isn't necessarily the ___best___ version control system, but it's very widely used. Git is also ___distributed___, which 
means that all the files you're tracking are on your local machine. When you upload your changes, you only have to upload the 
___deltas___. Your system is also more ___fault tolerant___, if a machine crashes you don't lose all your files. Microsoft 
even uses it. Yay.

Git works via a series of ___commits___. Commits are several different "snapshots", or versions of your work. These commits 
are organised in a tree structure, and store your data in a logical sequence. You can navigate this tree structure to get to 
previous/new versions of your files.

## Setting up git
The two first things you should do are run the commands to get Git set up for your user. The first is to set your global 
username: 
```
git config --global user.name "Michael Speiser"
```

And the second is to set your email:
```
git config --global user.email "speiserm@protonmail.com"
```

## Using Git
To use git, you need to first move to the folder you'd like to use as the location for your repository. Then you can 
initialize it with `git init` and git will create a hidden .git file that you don't really need to worry about.

It is highly recommended that you create a `readme` file to explain the purpose of your repo. If you make that file into a 
`readme.md` you can use markdown to format it.

`git status` tells you what branch you are on, how many commits you have, and what files are or are not being tracked.

To start making a commit, you have to first add files to the staging area. This is done with the `git add` command. Using `git 
add README.md` adds our `README.md` file to the staging area. You can also use selectors, such as `*.md` to select all .md 
files, or `git add .` to select all files and subdirectories.

Once you've added all the files you need to the staging area, you can use `git commit` to commit those files to the branch. If 
you don't use anything past `git commit`, a text editor will open where you can write your commit message. Make commit 
messages meaningful and descriptive of what that commit is changing within the repository. Using `git commit -m` allows you to 
add your commit message inline with the commit command.

`git log` shows you all the commits made to your current branch along with times and commit messages. Looking at the commit 
names (sha hashes) will allow you to select a specific commit to return to. Using `git checkout` changes your head (current 
commit) to whichever you select. For example, using
```
git checkout 0333d44
```
is enough to check out a commit with the hash of `0333d4433a08d716656f40488e867a5c38ce4ae9`.
