Day 3 Overview: Connecting Local Git to GitHub

Up until today, everything has been saved locally on your laptop inside .git. Today, we establish the bridge between your machine and GitHub (the cloud host) using industry-standard security.

Part 1: Create the Remote Repository on GitHub
1) Log into your account at GitHub.

2) In the top-right corner, click the + icon and select New repository.

3) Fill in the repository details:

   a) Repository name: azure-devops-lab

   b) Description: Azure Infrastructure and DevOps Sandbox

   c) Public / Private: Choose Private (recommended for infrastructure code) or Public.

   d) IMPORTANT: Leave "Add a README file", "Add .gitignore", and "Choose a license" UNCHECKED.

Why keep them unchecked? You already have a local repository with a history, .gitignore, and files. If you initialize GitHub with a README, the remote and local histories will diverge, causing a non-fast-forward merge conflict on your first push.

 4) Click Create repository.



 Part 2: Set Up Authentication (Personal Access Token)

 GitHub disabled raw password authentication for Git terminal operations. You must use either a Personal Access Token (PAT) or SSH Keys. We will use a PAT (HTTPS) as standard for pipeline service connections and initial setup.

 Generating your PAT:

 1. On GitHub, click your Profile Picture (top right) ----> Settings.
 2. Scroll to the bottom of the left sidebar and click Developer settings.
 3. Select Personal access tokens ----> Tokens (classic).
 4. Click Generate new token ----> Generate new token (classic).
 5. Set:
    a) Note: VS Code Local - Laptop
    b) Expiration: 30 days (or custom)
    c) Scopes (Permissions): Check the box for repo (Full control of private repositories).
 6. Generate Token
 7. CRITICAL: Copy the generated token immediately and save it in a secure password manager. You will not see it again.




 Part 3: Linking Local to Remote (git remote)

 Now, open your VS Code terminal inside your project directory (azure-devops-lab/Github_Day1_Day2 or root directory).


 Step 1: Add the remote URL:Run the following command, replacing <YOUR-USERNAME> with your actual GitHub username:

 git remote add origin https://github.com/<YOUR-USERNAME>/azure-devops-lab.git 

 Breakdown of this command:
 a) git remote add: Tells Git to store a pointer to a remote server.

 b) origin: The conventional alias name given to your primary remote server.

 c) https://...: The exact URL pointing to your cloud repository.

 Step 2: Verify the remote link

 To confirm that the git stored the URL correctly run:

 git remote -v

 Expected Output:

 origin  https://github.com/<YOUR-USERNAME>/azure-devops-lab.git (fetch)
 origin  https://github.com/<YOUR-USERNAME>/azure-devops-lab.git (push)

 Pro-Tip / Inspection Command:
 If you ever make a typo in the URL, fix it using:
 git remote set-url origin <CORRECT-URL>
 If you want to view detailed configuration for the remote, run:
 git remote show origin





 Part 4: Renaming Default Branch & First Push (git push)

 Step 1: Ensure Your Primary Branch is Named main
Check your current branch name:
git branch
If it says * master, rename it to industry-standard main:
git branch -M main