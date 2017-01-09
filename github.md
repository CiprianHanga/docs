## GitHub

### Create New Repo

- go to [GitHub Homepage](https://github.com):
- make sure you're logged in with your credentials
- append a ```/new``` at the end of the web address:

```
https://github.com/new
```

- follow the steps to create a new repo
- keep the page opened and proceed with the steps below

### Push an Existing Repository from Command Line

- go to your folder
- run these commands:

```bash
$ git init
$ git add *
$ git commit -m "first commit"
$ git remote add origin <insert your GitHub repo page>.git
$ git push origin master
```

- you are going to be asked:
	1. the GitHub username
	2. the GitHub password 

