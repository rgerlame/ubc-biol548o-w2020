## Introduction

### What is version control?

Version control is not any one tool or technique. Rather, it is a general approach to computational research that facilitates reliability and efficiency. In many ways, it is the computational analog of the tried, tested, and true practices for keeping good scientific notebooks in laboratory sciences.

Version control is pretty simple. For each different file that that is part of your project, you have:

* A system for keeping track of the file version  
* A complete history of all changes that have ever been made to the file  
* Ability to go back to any previous version

Researchers have developed many (perhaps idiosyncratic) ways to achieve (or partially achieve) these three goals that may involve many different tools. For example, many scientists use Dropbox and then save each version of a [file with a different name](http://phdcomics.com/comics/archive.php?comicid=1531).  

In this class, we will be using the software git and the online service GitHub as a version control system.


### Benefits of using git as a Version control system

* Track changes (*but better*)
    * Tracks every change ever made in groups called commits
        * Every commit stores the full state of all of your files at that time
    * Never lose anything
        * Revert or restore to any commit
        * Easily unbreak your code/data/manuscript
        * No more file name changes
* Collaboration
    * Work on things simultaneously
        * See what changes others have made
        * Everyone has the most recent version of everything

### How git works (broadly speaking)

* There is only file for each file (name isn't changed with each change to the contents)  
* The entire "tree" of changes is stored (in a hidden folder of your computer). Note that git can only keep track of changes inside text files; PDFs, .docx, .xls files, etc. can be added to git but the system can't see inside of them
* A (simplified) version of the git workflow is as follows:
    * Files are in your working directory (folder on your computer)
    * When you have made changes to a file and have saved it, using `git add` moves the changed files to the "staging area"
    * When you have complete a bundle of changes (we can talk about what this means together), you can use `git commit` to move the files from the staging area into your repository. These changes are now part of the history recorded on your local computer
    * When (and if!) you want to move files to an online repository (for example, GitHub), you can use `git push` to sync your online repository with the repository on your local machine. `git pull` does the opposite -- it syncs your local machine with the online repository.
    
## Tour of GitHub

* Commit history
* Branches
* Settings (private vs. public)
* Issues

## Practice with git

### First commits

#### Commit data

* Download the data file [Gaeta_etal_CLC_data.csv](https://lter.limnology.wisc.edu/sites/default/files/Gaeta_etal_CLC_data.csv) to your project directory.
* Add the data file to version control
* Two step process:

1. Add the data file (checkbox)
2. Commit it

* Git -> Select `Gaeta_etal_CLC_data.csv`.
* Commit with message. 
    * `Add fish size and growth rate data`
* History: 
    * One commit
    * Changes too large to see

#### Commit R script

* Read in data to new R script.

```
fish_data = read.csv("Gaeta_etal_CLC_data.csv")
```

* Save as `fish-analysis.R`. 
* Git -> Select `fish-analysis.R`. 
    * Changes in staged files will be included in next commit.
    * Can also see changes by selecting `Diff`
* Commit with message. 
    * `Start script comparing fish length and scale size`
* History: 
    * Two commits
    * See what changes were made to `fish-analysis.R`

### Building a history

* `fish-analysis.R` doesn't currently show on the `Git` tab
    * No saved changes since last commit
* Add some more code to `fish-analysis.R`
    * Create new categorical size column

```
library(dplyr)
fish_data_cat = fish_data %>% 
  mutate(length_cat = ifelse(length > 200, "big", "small"))
```

* Save `fish-analysis.R`.
* Now we see the file on the `Git` tab.
    * `M` indicates that it's been modified.
* To commit these changes, we need to stage the file.
    * Check the box next to `fish-analysis.R`.
* Commit with message.
    * `Add categorical fish length column`
* History: 
    * Three commits 
    * Each `fish-analysis.R` commit shows the additions we made in
      that commit.

* Modify this code in `fish-analysis.R` 
    * Change category cut-off size

```
fish_data_cat = fish_data %>% 
  mutate(length_cat = ifelse(length > 300, "big", "small"))
```

* Save file -> stage -> commit
    * `Change size cutoff for new column`
    * Green sections for added lines, red for deleted
    * Git works line by line.
        * The previous version of the line is shown as deleted.
        * The new version of the line is shown as added.


### Exercise 1

In `fish-analysis.R`, add a comment above the creation of `fish_data` describing what this code does. 

Commit this change to version control with a good commit message. Then check to see if you can see this commit in the history.


### Committing multiple files

* Commits can include multiple files at once
* Let's move our data file into a `data` subdirectory
* `New Folder` -> `data`
* Checkbox `Gaeta_etal_CLC_data.csv` -> `More` -> `Move`
* Change code to read from new subdirectory

```r
fish_data = read.csv("data/Gaeta_etal_CLC_data.csv")
```

* Changes to R script indicated by M
* Original datafile has a red D next to it which indicates "deleted"
* New, untracked, data directory
* git initially thinks we've deleted `Gaeta_etal_CLC_data.csv` and created a new `Gaeta_etal_CLC_data.csv` file in a new directory.
* Click on both the old and new files to stage them
* git then recognizes that we have moved (or renamed) the file by making the two files into one and marking this with an `R` for "rename".

* Commit: `Move data file into subdirectory`


### Exercise 2

After talking to a colleague, you realize that
`Gaeta_etal_CLC_data.csv` is only the first in a series of similar files that you will receive. To help keep track of files, you decide to number them. Rename the `Gaeta_etal_CLC_data.csv` file to `Gaeta_etal_CLC_data_1.csv` manually, using the Files tab in RStudio. You’ll also need to change the first line of `fish-analysis.R` so that the script will still work. 

To include all of these changes in a single commit, stage both data files and the saved R script and then commit with a good commit message. 

*Git will initially think you've deleted `Gaeta_etal_CLC_data.csv` and
created a new file `Gaeta_etal_CLC_data_1.csv`. But once you click on
both the old and new files to stage them, git will recognize this by making the two files into one and marking this with an `R`.*

### Git as a time machine

#### Experiment with impunity

<pre>
fish_data_cat = fish_data %>% 
  mutate(length_cat = ifelse(length > 300, <b>"large"</b>, "small"))
</pre>

* `Save` and show changes are staged
* <i class="fa fa-gear"></i> `More` -> `Revert` -> `Yes`

* Get previous state of a file
    * `History` -> select commit -> `View file @ ...`
    * Save file over current file
    * Copy and paste relevant piece into current file

#### Delete with impunity

* Both of these also work for deleted files
* Close the upper left window with the `fish-analysis.R`.
* Choose the `File` tab in the lower right window.
* Select `fish-analysis.R` -> `Delete` -> `Yes`
* Stage deleted file -> <i class="fa fa-gear"></i> `More` -> `Revert` -> `Yes`

## GitHub Remotes

* So far we've worked with a local `Git` repository.
* One of the big benefits of version control is easy collaboration.
* To do this, we synchronize our local changes with a remote repository called
  `origin`.
* Our remote repository is on GitHub. 
    * By far the most popular hosted version control site
    * Public and private hosted repositories
    * Private free for students and academics
	* https://education.github.com/
        * For the assignment, we're using private repositories that we made at the beginning.

### Push to a remote

* `Push` sends your recent commits to the `origin` remote.
* Before a `Push` your commits show in your local history but not on the remote.
* To `Push` to your remote, select the `Push` button at the top of the `Git`
  tab.
* Now your changes and commit history are also stored on the remote.


### Exercise 3

Pair up with one of your colleagues and invite each other to be collaborators on your 548O-username repo. (You can remove them as collaborators once you've completed the exercise; I am assuming you haven't put any sensitive material in there already -- if you have let your instructor know).

Each of you will `clone` each others Repositories.

* Go the *Tools* tab -> *Shell*
* Type `cd..` to change directory 
* Type `git clone <url-of-your-colleagues-repo>`

Now, you have access to their repository. Open up the .Rproj file in their repository.

Make a minor edit to their `README.md` file (maybe send them a note of encouragement for getting this far). Save, add, commit, and push the change to their remote repository.


### Pulling

* Big advantage to remotes is easy collaboration
* Avoids emailing files and shared folders where you are never sure if you
  actually have the most recent version
* Makes it easy to see what collaborators have done
* Automatically combines non-overlapping changes


### Exercise 4

`Pull` the changes your colleague made to your remote repository.

If you can see their changes to your `README.md` file, you can now remove them as a collaborator.


