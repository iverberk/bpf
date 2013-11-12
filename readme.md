# Branch-per-Feature Automation Scripts

## Background

Please visit [my blog post](http://www.ivoverberk.nl/blog/2013/11/08/git-workflow-automated-branch-per-feature/) for more background on the Branch-per-Feature workflow and the usage of these scripts.

## Installation

Put the 'git-release' and 'git-integrate' scripts somewhere in your path, that's all there is to it! :-)
You can also clone the repository somewhere on your computer and place symlinks to the scripts somewhere 
in your path. This way you can always pull new changes without having to manually update the scripts.

## Usage

### Git release

The release command will recreate the release branch based on the previous merges it finds. This allows you to rebuild the release branch with minimal effort and it can be done by any developer. It is also possible to remove any merges from the release branch with this script. For conflict resolution the 'rerere' option is used. This script can 'learn' from previous merges to enable automatic resolution of conflicts.

Invoke the release script by calling `git release`. This command takes several arguments:

-h, --help

Show a help message

-a BASE, --base BASE 

A reference to the commit from which the source branches are based. This defaults to 'master', but can be a tag like 'sprint_1-start'.

-b BRANCH, --branch BRANCH

Normally the script would delete the source branch and recreate it with the same name. This argument lets you specify the name for the new branch to be created and leave the source branch untouched.

-x EXCLUDE, --exclude EXCLUDE

Specify a comma seperated list of branches to be excluded. **NOTE: don't include any spaces between the comma's as they will interpreted as additional arguments.**

-l, --list

Process the source branch for merge commits and list them. When you specify this argument no changes to any branches will be made.

-D, --no-discard

By default the script will discard your local release branch and create it fresh from the remote location. This is almost always what you want because the release branch should be managed by the script. Any direct local modifications will be lost in the process. When you fetch the release branch from the remote you can also be sure that you have the latest version. This argument lets you specify that you want to keep your local release branch.

-r REMOTE, --remote REMOTE

Specify the remote repository to work with.

-i INTEGRATION, --integration

Specify a comma seperated list of (integration) branches to pre-fill the rerere cache with. This argument lets Git 'learn' previous resolutions from integration branches. This can be useful when you run into a merge conflict when (re) building the release branch. The same conflict might have been resolved on the integration branch and this argument lets you reuse that resolution.

-v, --verbose

Show errors when running Git commands. This is useful for debugging any errors that might occur.

-c, --rerere-cache

Enable pre-filling of the rerere cache. The 'git release' command will normally abort on any merge conflicts that occur during rebuilding of the release branch. This argument tells the script to first pre-fill the rerere cache based on the source branch and any integration branches that are specified with the -i or --integration options. Enabling this option will resolve most conflicts that you might encounter.


After the rebuild of the release branch has succeeded you can add additional feature branches and (force) push it to the remote.

### Git integrate

The integrate command will merge a source branch with an integration branch. It provides an easy way for developers to quickly merge their development work with an integration branch.

Invoke the integration script by calling `git integrate`. This command takes several arguments:

-h, --help

Show a help message

-s SOURCE, --source SOURCE

The source branch to merge with the integration branch.

-c, --check

Check if the source branch will merge cleanly with the integration branch.

-p, --push            

On a succesful merge push the integration branch to the remote

-d, --discard         

Discard the existing local integration branch and checkout a new integration branch from the remote if one exists. If no remote is specified with -r, the configured remote is used, or origin if none is configured.

-r REMOTE, --remote REMOTE

Specify the remote repository to work with.

-v, --verbose

Show additional (debug) information.
