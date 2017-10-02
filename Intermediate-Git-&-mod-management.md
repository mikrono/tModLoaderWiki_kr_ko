In this guide we will go over Git, what it is, how it works, and also how we can use Git for our mod source code.

## Foreword
It is important to note that you will be making your source code public (and your mod 'open-source') with this guide. If you do not wish to do so, you probably shouldn't follow along with this guide. (just read it) However, this guide will also explain how Git works so it may still be useful to you. Making your mod open-source comes with many notable benefits, such as:
* Other people will be able to help you, this includes adding new features or content to your mod, as well as fixing bugs or generally improving code
* Get easy feedback using the GitHub issues system, people can make issue threads and provide feedback there
* Your mod will be allowed to be put on the Open-source mods page on the wiki, giving your mod more visibility
For more info, visit the [Open-Source-Mods](Open-Source-Mods) page
* Provide more information using the GitHub wiki system, or even the [GitHub-pages system](https://pages.github.com/)

Most information of this guide has been adapted from [Microsoft](https://www.visualstudio.com/learn/what-is-git/) and making it available here, because their information is very clear compared to other sites.

## What is Git?
Git is a distributed version control system. Every developer of the project has a working copy of the code and full change history on their local machine. These fully-functional local repositories make it is easy to work offline or remotely. You commit your work locally, and then sync your copy of the repository with the copy on the server. This paradigm differs from centralized version control where clients must synchronize code with a server before creating new versions of code. Git is the most commonly used version control system.

## The very basics of Git
### Commits
Every time you save your work, Git creates a commit. A commit is a snapshot of all your files at a point in time. If a file has not changed from one commit to the next, Git uses the previously stored file. This design differs from other systems which store an initial version of a file and keep a record of deltas over time.

![](https://www.visualstudio.com/wp-content/uploads/2016/10/linear_straight_line.png)

Commits create links to other commits, forming a graph of your development history . You can revert your code to a previous commit, inspect how files changed from one commit to the next, and review information such as where and when changes were made. Commits are identified in Git by a unique cryptographic hash of the contents of the commit. Because everything is hashed, it is impossible to make changes, lose information, or corrupt files without Git detecting it.

### Branches
Each developer saves changes their own local code repository. As a result, you can have many different changes based off the same commit. Git provides tools for isolating changes and later merging them back together. Branches, which are lightweight pointers to work in progress, manage this separation.  Once your work created in a branch is finished, merge it back into your team’s main (or master) branch.

![](https://www.visualstudio.com/wp-content/uploads/2016/10/branching_line.png)

In this example, 'Some feature' is a feature made on a separate branch which can then later (when finished) be merged back into the master branch. This is useful for developing features of which you are not certain to be included in the final product.

### Staging and file states
Files in Git are in one of three states: modified, staged, or committed. When you first modify a file, the changes exist only in your working directory. They are not yet part of a commit or your development history. You must stage the changed files you want to include in your commit (you can omit files that you wish to commit separately). The staging area contains all changes that you will include in your next commit. Once you’re happy with the staged files, commit them with a message describing what changed. This commit becomes a part of your development history.

![](https://www.visualstudio.com/wp-content/uploads/2016/10/file_status_lifecycle.2.png)

Staging lets you pick which file changes to save in a commit so you can break down large changes into a series of smaller commits. When you reduce the scope of your commits, it’s easier to review the commit history to find specific file changes.

# These were the basics of Git...
Now let's move on to some tips when using Git. Further below we will discuss mod management with Git

## Commit frequently
All the time when working on your code you should think about when it is time to make a commit. Remember that your commits are only local as long as you don't push your commits to the public repository. It's a healthy habit to commit as frequently as possible, not just for the sake of management but also the assurance that those specific parts of code changes are saved and now in place in the development history tree.

## Be descriptive, but stay concise
Always make sure your commit names are concise. For example, if you've developed a new feature, just call it 'new feature: name' instead of 'feature name: it can do this, and potentially this and that'. If you want to be descriptive, use the description part of your commit.

## Make use of branches!
Many people are afraid of branches, don't be! They are the most amazing thing ever. Many times, it is useful to use branches even when not for new features specifically. Check the next tip.

## Use branches and pull requests for code review
It is very useful if you make use of the _awesome_ tools Git/GitHub provides you. This is something we often use for tModLoader development, but you can use it as well if you develop your product with multiple people. You can make a new branch and commit your changes to it, then create a new pull request to master from that branch. With  the new pull request, you can ask for feedback and code review from other collaborators!