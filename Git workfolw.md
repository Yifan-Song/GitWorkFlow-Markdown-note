# Git workfolw

标签（空格分隔）： introduction  宋逸凡516030910411

---

- &emsp;&emsp;**What?**

- &emsp;&emsp;**Why?**

- &emsp;&emsp;**How?**

![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015122301.png)

---
##1.What's Git workflow?

&emsp;&emsp;A Git Workflow, as the name suggests, is flow of work. More specifically, it's a recipe or recommendation for how to use Git to accomplish work in a consistent and productive manner. In other words, it's a rigorous branching model of project development.
&emsp;&emsp;Here's some most widely used workflow.

 &emsp;&emsp;*1. Git flow
 &emsp;&emsp;2. Github flow
 &emsp;&emsp;3. Gitlab flow*
 
**Git flow:**
 
There are two long-term branches of the project:
`Principal branch: master
Developing branch: develop`
The former is used to store the released version, which is distributed in a stable distribution at any time. The latter is used for daily development and storage of the latest development version.

What's more, there are three short-term branches of the project.
`Functional branch (feature branch)
Patch branch (hotfix branch)
Chaifa branch (release branch)`
Once they are developed, they are merged into develop or master, and then deleted.
 ![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015122302.png)
 
**Github flow:**
 
It has only one long branch, the `master`, which makes it very simple to use.
The official recommendation process is as follows.
![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015122305.png)

 - The first step: pull out a new branch from `master` without distinguishing functional branches or patch branches according to requirements.
 - The second step: after the development of the new branch, or when it is necessary to discuss, launch a pull request (PR for short) to `master`.
 - The third step: Pull Request is both a notification, a notice of your request, a dialogue mechanism, and a review and discussion of your code. In the process of dialogue, you can also submit code.
 - The fourth step: your Pull Request is accepted, merged into `master`, re deployed, the original branch that you pulled out was deleted. It can be deployed or merged

**Gitlab flow:**

The maximum principle of Gitlab flow is called "upsteam first", that is, there is only one principal branch master, which is the upstream of all other branches". Only code changes adopted by upstream branches can be applied to other branches.

Gitlab flow is divided into two cases to adapt to different development processes.

1. For the "ongoing" project, it proposes to build different branches of the environment beyond the master branch. For example, the "branch" developing environment is master, the "pre branch environment" is pre-production, "branch production environment" is production.
Is the pre development branch branch, branch and advance the "upstream" is the upstream production branch "". Code changes must be developed from upstream to downstream. For example, the production environment of the bug, then we must create a functional branch, put it into the master, confirmed that no problem, then cherry-pick to pre-production, this step is not a problem, before entering the production.
Only emergency conditions are allowed to skip upstream and merge directly into the downstream branch.
![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015122306.png)
2. For version release projects, it is recommended that every stable version pull out a branch from the master branch, such as 2-3-stable, 2-4-stable, and so forth.
Later, only patching the bug allows you to merge the code into these branches and update the small version number at this point.
![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015122307.png)

**What is a successful Git workflow?**

When evaluating a workflow for your team, it's most important that you consider your team’s culture. You want the workflow to enhance the effectiveness of your team and not be a burden that limits productivity. Some things to consider when evaluating a Git workflow are:

- Does this workflow scale with team size?
- Is it easy to undo mistakes and errors with this workflow?
- Does this workflow impose any new unnecessary cognitive overhead to the team?

---
##2.Why do we need Git workflow?
&emsp;&emsp;When developing a programe, we inevitably face the problem of version number control, which is similar to the industry companies. Every time you change the file, you need to add a new resume on the first page of the document of specific records change of the content. Assuming that many people operate at the same time. How high can you imagine the cost of maintenance?
&emsp;&emsp;So, version number control is the top priority of project development. Especially in the project of multi person cooperation. So here comes the question, Which control is the version number controlled? In a word, Git is the world's most advanced distributed version number control system. And git workflow is the specific way to achieve it.

![](https://raw.githubusercontent.com/quickhack/translations/master/git-workflows-and-tutorials/images/git-workflows-gitflow.png)

---
##3.How does it work?

Here's a Git usage specification process.

![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015080501.png)
**Step 1: build a new branch**
First of all, each time you develop a new function, you should create a separate branch.

`//Get the latest trunk code`
`$git checkout master`
`$git pull`
`//A new branch of myfeature development`
`$git checkout -b myfeature`

**Step 2: submit branch commit**
After the branch is modified, commit can be submitted.

`$git add --all`
`$git status`
`$git commit --verbose`

The all parameter of the GIT add command represents saving all changes (including new, modified, and deleted). Starting with Git 2, all is the default parameter for git add, so it can also be replaced by git add.
The GIT status command is used to view the files that have changed.
The verbose parameter of the GIT commit command lists the results of diff.

**Step 3: writing submission information**
When submitting commit, you must give complete and concise submission information, and here is a template.

`Present-tense summary under 50 characters
 * More information about commit (under 72 characters)
 * More information about commit (under 72 characters)
Http://project.management-system.com/ticket/123`

The first line is a summary of no more than 50 words, and then empty line, lists the reasons for the change, the main changes, as well as the need to pay attention to the problem. Finally, provide the corresponding URL (for example, Bug ticket)

**Step 4: synchronize with the trunk**
In the branch development process, you must always synchronize with the trunk.

`$git fetch origin`
`$git rebase origin/master`

**Step 5: merge commit**
After the completion of the branch development, it is possible to have a bunch of commit, but when merging into the trunk, often hope that only one (or at most two or three) commit, so that not only clear, but also easy to manage.
So, how can we merge multiple commit? It's going to use the GIT rebase command.

`$git rebase -i origin/master`

The I parameter of the GIT rebase command represents interaction (Interactive), and then git opens an interactive interface for the next step.
Here's an example of Tute Costa to explain how to merge commit.

`Pick 07c5abd Introduce OpenPGP and teach basic usage
Pick de9b1eb Fix PostChecker:: Post#urls
Pick 3e7ee36 Hey kids, stop all the highlighting
Pick fa20af3 git interactive rebase, squash, amend
Rebase 8db7e8b..Fa20af3 onto 8db7e8b `
`# Commands:`
`# P, pick = use commit`
`# R, reword = use commit, but edit the commit message`
`# e, edit = use commit, but stop for amending`
`# s, squash = use commit, but meld into previous commit`
`# F, fixup = like "squash" but discard this commit's log message`
`# x, exec = run (command the rest of the line using shell)`
`# These lines can be re-ordered they are executed from top; to bottom.`
`# If you remove a line here THAT COMMIT WILL BE LOST.`
`# However, if you remove everything, the rebase will be aborted.`
`# Note that empty commits are commented out`

The top interactive interface first lists the latest 4 commit of the current branch (the more the next is the next). There is an operation command in front of each commit, and the default is pick, which means that the row commit is selected, and rebase operation is performed.
There are a bunch of annotations below the 4 commit, listing the commands that you can use.

- Pick: normal selection
- Reword: select and modify the submission information;
- Edit: selected, rebase will be suspended, allowing you to modify this commit (refer to here)
- Squash: it will merge the current commit with the previous commit
- Fixup: the same as squash, but does not save the submission information of the current fixup
- Exec: execute other shell commands

Of the 6 commands above, squash and fixup can be used to merge commit. First change the verbs in front of the commit that need to be merged into squash (or s).

`Pick 07c5abd Introduce OpenPGP and teach basic usage
S de9b1eb Fix PostChecker:: Post#urls
S 3e7ee36 Hey kids, stop all the highlighting
Pick fa20af3 git interactive rebase, squash, amend`

After this change, the current branch will only have two commit left. The commit of the second and third rows will merge to the commit of the first row. The submission information will also include the submission information of the three commit.

`# This is a combination of 3 commits.`
`# The first commit's message is:`
`Introduce OpenPGP and teach basic usage`
`#This is the 2nd commit message:`
`Fix PostChecker:: Post#urls`
`#This is the 3rd commit message:` 
`Hey kids, stop all the highlighting`

Change the squash command of the third line to the fixup command.

`Pick 07c5abd Introduce OpenPGP and teach basic usage
S de9b1eb Fix PostChecker:: Post#urls
F 3e7ee36 Hey kids, stop all the highlighting
Pick fa20af3 git interactive rebase, squash, amend`

The result is the same, or will generate two commit, second lines and third rows of commit, are merged into the first line of commit. However, in the new submission information, the third line commit submission information will be annotated.

`# This is a combination of 3 commits.`
`# The first commit's message is:`
`Introduce OpenPGP and teach basic usage`
`#This is the 2nd commit message: `
`Fix PostChecker:: Post#urls`
`#This is the 3rd commit message: `
`# Hey kids, stop all the highlighting`

**Step 6: push to remote warehouse**
After merging commit, you can push the current branch to the remote warehouse.

`$git push --force origin myfeature`

The GIT push command adds the force parameter, because after rebase, the branch history has changed, is not compatible with the remote branch, may have to push forcibly

**Step 7: issue Pull Request**
After submitting to the remote warehouse, you can send Pull Request to the master branch, and then ask someone to code review, and confirm that it can be merged into master.

---
**Summary**

In this document we briefly introduced Git workflows. We took an in-depth look at a workflow with practical examples. Here's some points important about git workflows.

 - There is no one-size-fits-all Git workflow
 - A workflow should be simple and enhance the productivity of your team
 - Your business requirements should help shape your Git workflow