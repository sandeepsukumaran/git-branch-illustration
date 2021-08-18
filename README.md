# An illustration of some git branching principles


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

2. After the release, merge and tag for the release of `v1.1` as usual:
<img width="1029" alt="image" src="https://user-images.githubusercontent.com/10478594/129958154-9f2f562a-2828-466d-9b8e-a380fbbe52a3.png">
