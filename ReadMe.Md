Automating Git Tag Creation and Version Updates Using Jenkins 

Introduction 

This documentation explains how to automate Git tag creation and version updates using 
Jenkins for Every Successful Deployment in Production Server. Automating these processes 
ensures that version management is consistent, accurate, and efficient. However, manually 
creating and managing Git tags can be a time-consuming and error-prone task.

Goal 
To Track Every Successful Deployment in Production server in form of Git Tags
Overview 
1. Commit Message Patterns: Developers follow specific commit message patterns to 
indicate the type of change they've made.(Major, Minor, Patch)
2. Jenkins Pipeline: A Jenkins pipeline is set up to automate version updates and Git tag 
creation based on commit message patterns.
3. Version File: A version.txt file is maintained in the Jenkins workspace to track the current 
version of the software.
4. Increment Version Script: A script is used to increment the version based on the type of 
change indicated in the commit message.
5. Git Tag Creation: Jenkins creates and pushes Git tags to the repository based on the 
updated version.

Step-by-Step Guide 
1. Commit Message Patterns 
Developers follow specific commit message patterns to indicate the type of change:
 - [Major]: Indicates a major update (e.g., [Orderservice] Breaking changes: New version).
 - [Minor]: Indicates a minor update (e.g., [Orderservice] feature: added checkout feature).
 - [Patch]: Indicates a patch update (e.g., [Orderservice] bug fixed).
 
2. Jenkins Pipeline 
- A Jenkins pipeline is set up to automate version updates and Git tag creation.
- This pipeline listens for changes in the repository, triggered by new commits or pull 
requests.

 3. Version File 
- A `version.txt` file is maintained in the Jenkins workspace.
- This file contains the current version of the software, e.g., `1.0.0`.

4. Increment Version Script 
A script is used to increment the version based on the type of change indicated in the 
commit message.
- The script checks the commit message and increments the version accordingly:
 - `[Major]`: Increments the major version number. [Ex: V1.0.0 ---> V2.0.0]
 - `[Minor]`: Increments the minor version number. [Ex: V1.0.0 ---> V1.1.0]
 - `[Patch]`: Increments the patch version number. [Ex: V1.0.0 ---> V1.0.1]

5. Git Tag Creation 
Jenkins uses the updated version to create Git tags.
- For example, if the current version is `1.0.0` and a `[Minor]` update is detected, the script 
increments the version to `1.1.0` and creates a Git tag `v1.1.0`.

How It Works 
1. A developer makes changes the code and commits them with a commit message pattern, 
e.g., [PaymentService] Bug fix.
2. Jenkins detects the new commit and triggers the pipeline.
3. The pipeline reads the commit message, increments the version accordingly, and creates 
a Git tag for every Successful Deployment in Production
4. The updated version and Git tag are pushed to the repository.Version and release history 
are automatically managed.

Benefits 
- Consistent version management.
- Automatic Git tag creation.
- Accurate tracking of changes.
- Reduced manual work and human error.
- Facilitates collaboration and release management.

Conclusion 
Automating Git tag creation and version updates using Jenkins streamlines the versioning 
process, making it more efficient and reliable. This approach ensures that version 
management aligns with the development team's goals and provides an accurate history of 
software changes.
Following this, Each Successful deployment in Production can be tracked and seen in form 
of Git Tags