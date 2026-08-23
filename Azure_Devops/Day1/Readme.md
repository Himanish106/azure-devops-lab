Configure end to end Devops Setup.

Step 1: Provision the Azure Devops Organization and Repo

a) Action: Navigated to dev.azure.com, created an Azure Devops Project (Azure-Devops-Lab), and initialized a new empty git repository.

Why this was done: Enterprise Azure environments require cloud source repositories inside Azure Repos to directly bind code changes to Azure Boards (project management) and Azure Pipelines (CI/CD automated deployment engines).


Step 2: Multi-Remote Git configuration

a) Action: Added the Azure DevOps repository as an additional Git remote (azure) and appended it as a secondary push target under origin:

# Step 2a: Add standalone Azure remote
git remote add azure https://himanishdas23@dev.azure.com/himanishdas23/Azure-DevOps-Lab/_git/Azure-DevOps-Lab

# Step 2b: Configure dual push URL under origin
git remote set-url --add --push origin https://github.com/Himanish106/azure-devops-lab.git
git remote set-url --add --push origin https://himanishdas23@dev.azure.com/himanishdas23/Azure-DevOps-Lab/_git/Azure-DevOps-Lab

Why this was done: By default, Git only tracks a single remote URL per target. Configuring a dual-push URL under origin allows a single execution of git push origin <branch> to automatically mirror commits to both GitHub and Azure DevOps simultaneously, eliminating redundant manual commands.


Step 3: Implement Enterprise branch policies

Action: Configured branch policies on the main branch under Repos--->Branches--->Branch Policies.

a) Require a minimum number of reviewers: Enforces code review approvals before merging.

b) Check for linked work items: Mandates linking commits/PRs to Azure Boards tasks or user stories.

c) Check for comment resolution: Ensures active reviewer feedback threads are marked resolved.

d) Limit merge types (Squash Merge): Melts noisy development commits into a single clean commit on main.

Why this was done: Protects production code from unvetted pushes, maintains an auditable development trail for compliance, and keeps the repository history linear and readable.



Step 4: Resolve Branch Policy Push Blocks & Configure Bypass Permissions

Action: Encountered error TF402455 (push rejected because branch policies enforce pull requests). Configured policy bypass permissions:

a) Went to Project Settings ----> Repositories -----> Azure-DevOps-Lab ----------> Security.

b) Selected your user identity (Himanish Das) and set Bypass policies when pushing to Allow.

Why this was done: In standard operations, developers submit Pull Requests. However, administrative/break-glass roles require bypass authority to push emergency fixes directly from the local terminal without failing automated policy blocks.


Step 5: Dual-Branch Synchronization and Push Verification

Action: Pushed your working branches and version tags across cloud systems

git push origin main
git push origin dev
git push origin --tags

Why this was done: Synchronized commit histories across GitHub and Azure DevOps, ensuring local branches, remote trackers, and production release milestone tags remain identical across all platforms.