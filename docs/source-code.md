# Source code

* We use `git` for version control:
    * <{{ config.repo_url }}>
    * <{{ extra.mirror_url }}> (mirror)
* Our development branch is called `master`
* Prefix feature branch names with the issue ID (e.g. `4632_shift_fx`)
* Rebase with `-i --autosquash` before merging to preserve linear history
* Use non-fast forward (`--no-ff`) merges to track commit origin

## Contributing

We require an issue (or pull request) for any code contribution, except for the following, which can be committed directly to the `master` branch:

* Translations
* Documentation
* Infrastructure (`.github`)

The commit message for the first patch in the series should begin with the following header:

```
Ticket #<github_issue_number>: brief summary of the changes
```

### Signing off commits

!!! note
    DCO sign-off is different to "commit signing" using something like PGP or `gitsign`!

We require all contributors to acknowledge that they own the rights to the code they contribute by signing their commits with `git commit --amend -s` (adding a `Signed-off-by:` line to the commit message). This signifies that they abide by the [Developer Certificate of Origin, Version 1.1](https://developercertificate.org), which is also used by the Linux kernel.

### Branch lifecycle

!!! tip
    Don't forget to update the changelog as tracked on the current `NEWS-4.8.XX` wiki page!

1. Without PR
    - [ ] Assign issue to yourself
    - [ ] Update milestone
    - [ ] Link a branch
    - [ ] When ready for review:
         1. Add the label `state: in review`
         2. `@mention` the reviewers
    - [ ] Address comments with `git commit --fixup`
    - [ ] When approved, add a `state: approved` label
    - [ ] Make sure that CI is green and `git rebase -i --autosquash`
    - [ ] Proceed with the merge 
    - [ ] Manually delete the feature branch :material-alert-outline:
2. With PR
    - [ ] Open in draft mode if not ready yet
    - [ ] Switch to normal when ready for review
    - [ ] Update milestone
    - [ ] Invite reviewers
    - [ ] Address comments with `git commit --fixup`
    - [ ] Secure approvals
    - [ ] Make sure that CI is green and `/rebase`
    - [ ] Proceed with the merge 
    - [ ] Feature branch is deleted automatically

### Code review

!!! tip
    To substantially increase the likelihood that your changes will be accepted, be sure to add **tests** for the code you touch.

Contributions should be reviewed according to the following criteria:

*Motivation*
:   It should be clear what the purpose of the proposed changes is.

*Tests*
:   Changes should be accompanied by tests that document that the desired goals have been achieved.

*Code quality* (TODO see HACKING)
:   
    * Code is split into logically independent commits
    * Changes are consistent with the rest of the code base
    * No large whitespace changes intermixed with logic changes

*Technical evaluation*
:   The code is a valid approach to the problem.

## Cleanup branch

For every release, we create a cleanup branch like `4633_cleanup` to aggregate small code changes, connected to the corresponding release ticket (e.g. [#4633]({{ config.repo_url }}issues/4633)).

* The cleanup branch should be created as needed and merged back in periodically (currently about once a month).
* Delete the cleanup branch after merging it into the main branch, and re-create it later with the same name if necessary.
* The cleanup branch should be merged into the main branch no later than a few weeks before the release, to allow enough time for the changes to be tested by developers and users.
