## Git Linux (openSUSE)


### Install Git

```bash
$ sudo zypper install git-core
```


### First Steps

#### Configuration

- to add your credentials to the repo:

```bash
git config --global user.name "Ciprian Hanga"
git config --global user.email "ciprianhanga@gmail.com"
git config --global --list
```

#### Initialization

- to turn any directory into a Git repository, run ```git init```
- ```git init``` creates a hidden folder ```.git``` at the top level of your project (where the ```git init``` was run)
	- this is different from CVS or SVN, which create subfolders within each of your project directories
- this hidden folder is completely maintained by Git and you shouldn't touch it

#### Adding Files to the Project

- adding files to the project needs to be explicit -- the reasoning behind this decision is to separate scratch/temp files from important files that you really want to track
- use ```git add``` *file* to add a file to the repository:

```bash
$ git add index.php
```

	- if you want to add all the content of the current folder to the repo, you can use ```git add .```, ```.``` standing for the current directory
- the ```git add``` command only *staged* the file, which is an interim step before committal -- Git separates the ```add``` and ```commit``` steps to avoid volatility
	- this should be likened to the idea of putting the files *en queue* for the next commit
	- the ```git status``` command reveals which files are staged ATM

- in addition to actual changes to the directory and to file contents, Git records several other pieces of metadata with each commit, including a log message and the author of the change


