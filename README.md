# An illustration of some git branching principles


### Case 1:

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
After this, the `HEAD` of the `release` branch is tagged to mark the new release point.
