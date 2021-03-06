# Rebase dashboard

## Overview

GitSavvy's rebase dashboard lets you:

- view a list of commits between your HEAD and the inferred start of the branch;
- re-order commits in your branch;
- squash commits in your branch;
- edit commit messages for commits in your branch; and
- rebase on top of any local or remote ref.

Re-ordering and squashing take a single key-press, with no further interaction required.  Editing is similar, with the additional step of modifying the commit message and submitting.

Rebasing is a guided process.  If merge conflicts occur in the middle of the rebase, specifics will be displayed and the conflicting file(s) will be shown below the commit in question.  At that time, you can resolve the changes and continue with the rebase.

Commits are shown in order of application, where the base commit (or inferred root of your branch) is displayed first, and the last commit made is displayed last.


## Actions

### Manipulating commits

#### Squash commit with next (`q`)

The selected commit will be squashed with the commit below it.  The two commit messages will be combined into the one below.

**Note:** This command cannot be performed while a rebase is ongoing.

#### Squash all commits (`Q`)

All commits listed in the view, from the base to HEAD, will be squashed into a single commit.  All commit messages will be combined into the single commit that results from this operation.

**Note:** This command cannot be performed while a rebase is ongoing.

#### Edit commit message (`e`)

When this command is run, a new view will be displayed containing the commit message from the selected commit.  Once you've completed any desired edits, submit the changes using the instructions in the view, and the commit's message will be updated.

**Note:** This command cannot be performed while a rebase is ongoing.

#### Move commit down (`d`)

This will move the commit from its current location in the commit chain to be positioned immediately after the next commit.  This command may fail if a merge conflict would arise from the operation.  If commit re-ordering is necessary in this case, it is best to drop into the terminal to complete the operation from there.

**Note:** This command cannot be performed while a rebase is ongoing.

#### Move commit up (`u`)

This will move the commit from its current location to be positioned immediately before the previous commit.  This command may fail if a merge conflict would arise from the operation.  If commit re-ordering is necessary in this case, it is best to drop into the terminal to complete the operation from there.

**Note:** This command cannot be performed while a rebase is ongoing.

#### Show commit (`w`)

This will display the selected commit data and meta-data in a new window, including author, date, diff, message, etc.


### Rebasing

#### Define base ref for the dashboard (`f`)

When initially opening the rebase dashboard, GitSavvy will attempt to determine the root commit of the branch, i.e. the HEAD when you typed `git checkout -b BRANCH_NAME`.  This commit is used as the starting point for the information that is displayed.

In most cases, this will default to `master`.  However, if you would like to compare the current branch against something other than local `master`, using this command will allow you to make that selection. You can override the default per-project by adding a `rebase_default_base_ref` to your `.sublime-project` file:

```json
{
    "folders": [
        {
           "path": "XYZ"
        }
    ],
    "settings": {
        "rebase_default_base_ref": "develop"
    }
}
```

#### Rebase branch on top of other branch (`r`)

When running this command, you will be prompted for the branch on top of which you'd like to rebase.  Once selected, a rebase operation will be initiated (`git rebase BASE_REF`).

If any merge conflicts occur while rebasing, the state of the operation will be reflected in the dashboard, with successful rebased commits showing a check mark, and the commit with the current merge conflict showing an X as well as a list of files with conflicts.

#### Continue rebase (`c`)

If a merge conflict occurs, you should resolve the conflict with one of the available options (listed below).  Once that is done and all conflicts are addressed, no conflicting files should be displayed in the dashboard.  At that time, run this command to continue the rebase operation.

#### Skip conflicting commit (`k`)

If a merge conflict occurs, sometimes it is acceptable to skip the offending commit.  Use this command to make that choice.

#### Abort rebase (`A`)

If you are unable to address merge conflicts that arise, or wish to abort the incomplete rebase for any reason, use this command to revert to the state before you began.

### Managing conflicts

**Note:** These key-bindings are only available mid-rebase when conflicts occur.

#### Open file (`o`)

Selecting a conflicting file and running this command will open the selected file in a new view, ready for you to make modifications.

#### Stage file in current state (`s`)

If you've addressed merge conflicts manually or with an external merge tool, select the file in question and run this command to add the file to the index.  This command (or one of the "use version X" commands) must be run before continuing a rebase when conflicts occur.

#### Use version from your commit (`y`)

When a conflict occurs, selecting the file in question and running this command will indicate to Git that you'd like to use the version of the file that was originally in your branch.

This is equivalent to `git checkout --theirs -- FILE_PATH`.  This is correct because, during rebase, "ours" refers to the branch you're rebasing on top of.

#### Use version from new base (`b`)

When a conflict occurs, selecting the file in question and running this command will indicate to Git that you'd like to use the version of the file that is found in the new base commit.

This is equivalent to `git checkout --ours -- FILE_PATH`.  This is correct because, during rebase, "ours" refers to the branch you're rebasing on top of.

#### Resolve conflict with external merge tool (`M`)

When a conflict occurs, selecting the file in question and running this command will launch the external merge tool to resolve the conflict.  Once complete, if the merge tool does not automatically do so, press `g` to add the resolved version to the index.
