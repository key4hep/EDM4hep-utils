# Contributing to EDM4hepUtils

Please read these guidelines if you want to contribute to the EDM4hepUtils project.


## Git workflow

For EDM4hepUtils we would like to follow a so called "No Switch Yard" (NoSY) workflow.

In essence this means that you develop your (small) new feature in a dedicated
*feature branch* that is kept up to date with the master branch until you create
a PR, as we only allow *rebase merges* for PRs.


### Example workflow

- checkout a copy of EDM4hepUtils from the origin at Key4HEP:

      git clone https://github.com/key4hep/EDM4hep-utils.git
      cd EDM4hep-utils

- create a fork of the repository on the Github web page
  - if you have not yet done so earlier

- add your fork as remote downstream using your Github username

      git remote add downstream  https://<yourUserName>@github.com/<yourUserName>/EDM4hep-utils.git

- create a new feature branch; choose a meaningful name

      git checkout -b <myNewBranch>
	
- make the changes to existing files (or add new ones) and frequently keep up to date with the master:

      git fetch origin; git rebase origin/master
	

- after having committed everything to your new branch, push it to your fork of EDM4hepUtils:

      git push downstream <myNewBranch>

- reload your own github website (`https://github.com/<yourUserName>/EDM4hep-utils`)
  - you should see your commit
  - now you can create a pull request on the web site


### Release Notes 

Please make sure you fill in meaningful release notes in the comment field that is
provided at the Github web page when creating the PR, e.g.

```

BEGINRELEASENOTES
- updated documentation
    - add guidelines for contributing
- reverted some name changes in tests/examples
    - `read-one` now again called `read`

ENDRELEASENOTES



```


