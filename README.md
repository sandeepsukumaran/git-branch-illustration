# An illustration of a three branched approach to maintain codebases

### General guidelines
The following cases can be distilled into a few key points:
1. When working _alone_ on a feature branch, bring in changes from the base branch often by _rebasing_ the feature branch _onto_ the base branch.
2. In case of pair programming, one person can assume responsibility to freeze and update the remote with a force push, after which everyone else checks out a fresh copy of the branch.
4. Always use pull requests to contribute code to shared/busy branches. Prefer merging PRs using the rebase and merge strategy to retain granularity while maintaining a linear history. The flipside of this coin - tailor your PRs to have meaningful commits (squash/rebase locally, edit commit messages).
5. **NEVER** force-push to a branch you do not "own".

<hr/>

## Illustrations
This document assumes the three branches in question are:
1. `main` - the long running working branch
2. `release` - the long runnin release branch. At any point, the `HEAD` of release is present in production environments. Contains tags that indicate the history of release points.
3. `release-M.m.p` - the ephemeral release candidate branch. Created when a release is planned, branching off from the `HEAD` of `release`. Changes to it are brought in from `main` via PRs from cherry picks. After a release, this is merged back into `release` and deleted.

### Case 1: The happy path

#### Context:
`v1.0` is the latest release point and is the current `HEAD` of the `release` branch. The `main` branch has more changes, some of which have to be released in the next release:
<img width="1029" alt="image" src="https://user-images.githubusercontent.com/10478594/129952605-43eb121a-35ee-4c44-810b-5099a153c561.png">

#### Goal:
Changes introduced as `file2.txt` and `file4.txt` are to be released.
#### Process:
The release candidate branch, `release-1.1` is created from the last release point, with the required changes *cherry picked* from `main`:
<img width="1029" alt="image" src="https://user-images.githubusercontent.com/10478594/129953386-3faf9b20-6acf-41f0-b34c-7b91a134eb77.png">

After the release, the release candidate branch, `release-1.1` is merged into the base `release` branch using the **Rebase and Merge** strategy:
<img width="1029" alt="image" src="https://user-images.githubusercontent.com/10478594/129954724-75e02c42-2279-4709-ab62-f94ca572c4a5.png">
After this, the `HEAD` of the `release` branch is tagged to mark the new release point. The release candidate branch is deleted.

<hr/>

### Case 2: An unplanned release

#### Context:
`v1.1` is the latest release and is the current `HEAD` of the `release` branch. The `main` branch has more changes, some of which have to be released in the next release. A release candidate branch for the next planned release has been created `release-1.2`, with the requisite change - addition of `file5.txt`:
<img width="1029" alt="image" src="https://user-images.githubusercontent.com/10478594/129956573-8981ed39-688f-47b2-afb2-bafc1de85d68.png">

However, a bug has been reported, necessitating an urgent patch release, namely `release-1.1.1`. The change involves a correction of `file2.txt`.

#### Goal:
Release the patch changes from last stable point and update the subsequent release candidate to include this change.

#### Process:
1. Checkout the new release candidate from the `HEAD` of the `release` branch and add changes to it. This example assumes the fix has been made in `main` and is being cherry picked to the release branch. Alternatively, the cherry pick can be in the other direction. Either approach can require a merge conflict resolution.
<img width="1029" alt="image" src="https://user-images.githubusercontent.com/10478594/129957441-03255ed3-ef55-4980-92ed-c0f16cdee4fe.png">

2. After the release, merge, delete, and tag for the release of `v1.1` as usual:
<img width="1029" alt="image" src="https://user-images.githubusercontent.com/10478594/129958154-9f2f562a-2828-466d-9b8e-a380fbbe52a3.png">

3. **STOP ALL CONTRIBUTION TO** `release-1.2`. Ideally, no updates are done to `release-1.2` once `release-1.1.1` is known to be required. When short on time, changes _can_ continue to occur on `release-1.2` until the base `release` branch is updated. At this point, `release-1.2` is to be updated.

4. **SELECT A SINGLE CONTRIBUTOR** to update the old release candidate `release-1.2`. Let's refer to this person as the `assignee`.

5. The `assignee` pulls all changes to `release` and `release-1.2` to their local repo. N.B: This pull of `release-1.2` "freezes" that branch. No updates to the branch after this will be honored.

6. The `assignee` rebases `release-1.2` _onto_ `release`, resolving any merge conflicts as needed. At this point, their local copy of `release-1.2` is up-to-date, while the remote is still out-of-date:
<img width="1029" alt="image" src="https://user-images.githubusercontent.com/10478594/129959331-b711cc51-9920-4db8-aac6-f1cdf2cbc398.png">

7. The `assignee` **FORCE PUSH**es `release-1.2` to remote. This rewrites the tree for `release-1.2` on the remote. The branch is now open to contributors again:
<img width="1029" alt="image" src="https://user-images.githubusercontent.com/10478594/129959679-0021fa83-2119-4f81-88c7-22a3a9dbd355.png">

8. **ALL OTHER CONTRIBUTORS** now have an out-of-date _local_ copy of `release-1.2`, as shown:
<img width="1029" alt="image" src="https://user-images.githubusercontent.com/10478594/129960038-305bc5dd-117a-48ab-84e6-84044347d235.png">

To update, **delete** the local copy of `release-1.2` and **checkout a _fresh_ copy** of `release-1.2` from remote:
<img width="1029" alt="image" src="https://user-images.githubusercontent.com/10478594/129960382-751422f8-f94e-4ed0-99aa-fa845e3b1f1b.png">

Everyone now has the up-to-date version of the release candidate.
