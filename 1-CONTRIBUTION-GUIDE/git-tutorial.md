# Microsoft Power Platform Center of Excellence (CoE) Starter Kit

## Git tutorial

We're following basic GitHub Flow. If you have ever contributed to an open source project on GitHub, you probably know it already - if you have no idea what we're talking about, check out [GitHub's official guide](https://guides.github.com/introduction/flow/).

Since some Power Platform components are authored within the service and not on a local machine, some of the steps are different than you may be familiar with. Power Platform components authored within the service are packaged and deployed using [*Solutions*](https://docs.microsoft.com/en-us/powerapps/maker/common-data-service/solutions-overview). *Solutions* can be exported and unpacked into a source control system.  The CoE Starter Kit uses *Solutions* to distribute the modules we release.  This repository contains a folder at the root with the name of each *Solution*.  For example, you will find the *CenterofExcellenceCoreComponents* folder in the repository represents the source code for the *Solution* we release name *CenterofExcellenceCoreComponents*.  Each *Solution* in this repository has a branch for then *vnext* release number of each *Solution* using the following naming convention: *SolutionName-MajorVersion.MinorVersion*.  For example, the *1.71* release of  *CenterofExcellenceCoreComponents* has a branch named *CenterofExcellenceCoreComponents-1.71*  

Here's a quick summary of how to use the GitHub Flow to contribute to a Solution in this repository:

+ Fork the repository
+ Follow the instructions in the [**Setting up the DevOps Starter Kit**](/devops-setup.md) guide
    + This provides tools to help make your Power Platform development loop more productive
+ Use the *PowerOps for Azure DevOps* app to help you
    + Get the latest *Solution* deployed to your development environment
    + Author your contributions
    + Create a branch in your fork of the repository based off the current code in the *vnext* branch
    + Commit your changes to your working branch
+ Visit this repository in GitHub and create a Pull Request

For a detailed tutorial, please check out the following information.

You probably heard of Git before, but it's possible that you haven't used it. Contributing to this repository doesn't require masterful Git skills, but you will need a few basics. This small tutorial will go over all the steps, taking you from zero to contributor.

This guide assumes you're new to git and that you're using a Windows computer. If you're using Linux OSX, its very similar, with the exception of Windows-specifics such as installation of git.

#### Table of Contents
- [Git Tutorial for ARM Template Submissions](#git-tutorial-for-the-pct-arm-templates)
    - [Get Git](#get-git)
    - [Fork the Repository to your Account](#fork-the-repository-to-your-account)
    - [Clone the CoE Starter Kit repository to your machine](#clone-the-coe-starter-kit-repository-to-your-machine)
    - [Creatine a new Branch for your contribution and Push your Changes](#create-a-new-branch-for-your-contribution-and-push-your-changes)
    - [Make a Pull Request](#make-a-pull-request)
    - [Updating Pull Requests](#updating-pull-requests)
    - [Squashing Commits](#squashing-commits)
    - [Syncing Your Fork](#syncing-your-fork)

### Get Git
If you don't have Git installed, head over to the official [Git Download page and download it](https://git-scm.com/download/win). While you won't need to use Git from the command line often, there are times where you might.

#### Ensure a Safe Push Behavior
Git has multiple ways of pushing - and changed the default behavior a few years ago. `git config --global push.default` configures what branches will be pushed to the remote. The default in Git 2.0 is `simple` meaning that whenever `git push` is run, only the current branch is pushed. Prior to version 2.0, the default behavior was `matching`, meaning that a `git push` would push all branches with a matching branch on the remote.

If you're installing a fresh version of Git, you're fine. If you want to make super-duper sure that everything will be okay, run the following command:

```
git config --global push.default simple
```

#### A Word About Git Clients
Many users are initially put off by the idea of having to work with Git through the command line. I distinctly remember not even considering the PowerShell/Terminal and instead downloading one of the many GUI tools that are available. As someone who went through the experience of trying to use a Git GUI without really understanding what Git is doing on the command-line-level, let me tell you: It will end in tears.

In fact, I find it a lot easier today to work with the command-line. Git will do *exactly* what you tell it do - each step will be obvious to you. GUIs often try to combine multiple commands together into one fancy button, which will surely blow your whole project up, if you don't understand what's going on under the hood. For those reasons, I *heavily* recommend sticking with the command-line.

### Fork the Repository to your Account
Before we can get started, you need to register with GitHub. Either create or login into your account. Then, head over to the [microsoft/coe-starter-kit](https://github.com/microsoft/coe-starter-kit) repository and click the little 'fork' button in the upper right.

![Fork the repo](images/git1.png)

This will create a copy of the repository as it exists in the `microsoft` organization (hence called microsoft/coe-starter-kit) in your own account.

### Clone the CoE Starter Kit repository to your machine
Visit your fork (which should be at github.com/{your_name}/coe-starter-kit) and copy the "HTTPS Clone URL". Using this URL, you're able to `clone` the repository, which is really just a fancy way of saying "download the whole repository, including its history and information about its origin".

![Clone the repo](images/git2.png)

Now that you have Git installed, open up PowerShell. If everything worked correctly, you should be able to run `git --version`. If that works, navigate to a folder where you'd like to keep the `coe-starter-kit` repository (and hence, all your arm templates). To get a copy of your fork onto your local machine, run:

```
git clone https://github.com/{YOUR_USERNAME}/coe-starter-kit
```

This should generate output that looks roughly like this:

```
Windows PowerShell
Copyright (C) 2015 Microsoft Corporation. All rights reserved.

C:\GitHub\devkeydet> git --version
git version 1.9.5.msysgit.1
C:\GitHub\devkeydet> git clone https://github.com/devkeydet/coe-starter-kit
Cloning into 'coe-starter-kit'...
remote: Counting objects: 1027, done.
remote: Compressing objects: 100% (4/4), done.
Receiving objects: 100% (1027/1027), 16.02 MiB | 9.11 MiB/s, done.
emote: Total 1027 (delta 0), reused 0 (delta 0), pack-reused 1023R
Resolving deltas:   5% (30/531)
Resolving deltas: 100% (531/531), done.
Checking connectivity... done.
C:\GitHub\devkeydet>
```

You can run `explorer coe-starter-kit` to open up the folder. If you are using Visual Studio Code, you could also run 'code coe-starter-kit'.  All the files are there - including the history of the whole repository. The connection to your fork (`your_name/coe-starter-kit`) is still there. To confirm that, `cd` into the cloned folder and run the following command, which will show you all the remote repositories registered:

```
git remote -v
```

The output should be:

```
C:\GitHub\devkeydet\coe-starter-kit [main]> git remote -v
origin  https://github.com/devkeydet/arm-templates (fetch)
origin  https://github.com/devkeydet/arm-templates (push)
```

As you can see, we're connected to your fork of the arm-templates (called "origin"), but currently not connected to the upstream version living in `microsoft/arm-templates`. To change that, we can simply add remotes. Run the following command:

```
git remote add upstream https://github.com/microsoft/coe-starter-kit
```

Entering `git remote -v` again should give you both repositories - both yours (called "origin") and the one for the whole team (called "upstream").  We'll use "upstream" later.

### Create a new Branch for your contribution and Push your Changes

[TODO: Writeup and screenshots of how to do this with PowerOps for Azure DevOps]

### Make a Pull Request

Now, head over to the [microsoft/arm-templates](https://github.com/microsoft/coe-starter-kit) repository. In most cases, GitHub will pick up on the fact that you just pushed a branch with some changes to your local fork, assuming that you probably want for those changes to end up in the upstream branch. To help you, it'll show a little box right above the code asking you if you want to make a pull request.

![Make a Pull Request](images/git3.png)

Click that button. GitHub will open up the 'Create a Pull Request Page'. It is probably a good idea to notify potential reviewers in your pull request. You can do this by doing an @mention to the maintainer of the repository.

As soon as you hit the 'Create Pull Request' button, it'll show up in the list of pull requests and trigger some automated validation.

### Updating Pull Requests

Once people have reviewed your pull request, it is very likely that you want to update it. Good news! Whenever you update the branch in your fork, your pull request will be automatically updated.

Let's look at a scenario: You just committed changes to your *Solution* using the *PowerOps for Azure DevOps* app and pushed to the working branch in your fork, went to GitHub, and created a Pull Request.

To update the PR with additional changes, simply use the *PowerOps for Azure DevOps* app to commit/push again.  Make sure you are pushing to the same branch you pushed to last time.

[TODO: Screenshot(s)]

When we're ready to merge your Pull Request, we will "squash your commits". The next section explains what this means.

### Squashing Commits
You should commit/push often. It's a great backup and safety net in case you mess up. At the same time, we don't want the changes we bring into this repository to contain all your commits.  We want all your changes to become one commit. There are obvious exceptions to this rule (like epic projects - think 'Windows support for Docker'), but your contributions should most definitely be just one commit in this repo. For that to happen, we need to rewrite history.  Don't worry, we'll do it for you when we accept your PR and merge it into this repository.

### Syncing Your Fork

This is where you will typically need to use Git from the command line.  After some time, you'll find that your fork has become out of sync with the upstream repository (as others contribute and their changes get merged in). To sync your fork up, you just need to do a local merge and then push those changes. There are a few different ways to do this, but this is the method [recommended by GitHub](https://help.github.com/articles/syncing-a-fork/):

```
# Get all of the latest from upstream for all branches
git fetch upstream
# Now make sure you're on main - the main branch for this project
git checkout main
# Merge in the changes from upstream (note: this commits locally)
git merge upstream/main
# And then push those to your fork on GitHub
git push
```

Now you can start from a clean state, knowing your next contribution will be the only divergence from `upstream` you'll have to deal with.

**Based on an awesome contribution guide at [Azure Resource Manager QuickStart Templates Git tutorial](https://github.com/Azure/azure-quickstart-templates/blob/master/1-CONTRIBUTION-GUIDE/git-tutorial.md)**
