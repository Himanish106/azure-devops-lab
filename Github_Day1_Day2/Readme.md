----------------------------- DAY 1 ---------------------------------------------

Prerequisites: VS CODE installation and Git bash and GITHUB account

a) Configuring your GIT Identity:

i) Configure your name: git config --global user.name "Your Name"
ii)Configure your email: git config --global user.email "your.email@example.com"
iii) Set your default branch name: git config --global init.defaultBranch main

Verify your configurations: git config --list

Look through the printed list for user.name, user.email, and init.defaultbranch=main.

b) Reason for configuring all of these: 

Why Did We Configure user.name and user.email?
In a real company, you will never work on code alone. A cloud environment or pipeline script is edited by multiple engineers across a team.

Every time you save a snapshot in Git (called a commit), Git attaches a digital "signature" to it using that name and email.

This answers three crucial real-world needs:

Auditability & Accountability: If someone pushes a script that accidentally deletes a subnet in Azure at 2:00 AM, the team needs to know who authored that specific change so they can understand what was changed and why.

Connecting to GitHub: When you push your code to GitHub later, GitHub matches the email on your local Git snapshots to your GitHub account profile. This ensures your contributions show up correctly on your profile and team repositories.

Collaboration: In tools like VS Code (with extensions like GitLens), your teammates can hover over any line of code and instantly see who wrote it and when.

Why Did We Run init.defaultBranch main?
When you create a new Git repository, Git creates a primary default line of history called a branch.

Historically, Git named this default branch master.

The tech industry (GitHub, Azure DevOps, GitLab, AWS) shifted to using main as the universal standard name.

Setting init.defaultBranch main ensures that every new repository you create on your machine automatically uses main right from the start, avoiding naming conflicts when syncing with GitHub.



Welcome to Day 1 of Week 1!

Now that your prerequisites are complete, today we learn how Git actually works on your computer, starting with a real-world story and your very first project folder.

Day 1, Part 1: The Real-World Problem Git Solves
Imagine you are managing an Azure deployment script named deploy_vm.sh.

Without Git, your project folder eventually looks like this mess:

deploy_vm.sh

deploy_vm_v2.sh

deploy_vm_final.sh

deploy_vm_final_FIXED.sh

deploy_vm_DONT_DELETE_working.sh

If a script breaks production at 3:00 AM, you have no idea:

Which file is actually running in production.

What exact line of code was changed to break it.

How to safely undo the mistake without losing all your recent work.

With Git: You keep one single file (deploy_vm.sh). Git acts like a time machine running quietly in the background, recording every snapshot (change) you save over time. You can view past versions or roll back anytime with a single command.

Hands-on Step 1: Create a Project Folder & Open VS Code
Let's build a clean space on your computer for our course projects.

Open Git Bash (Windows) or Terminal (Mac/Linux).

Run these two commands to create a new folder named azure-devops-lab and navigate inside it:

mkdir azure-devops-lab
cd azure-devops-lab
Now, open this folder inside Visual Studio Code by running:

code .
(Note: The dot . means "this current folder". If VS Code doesn't open automatically, open VS Code manually, click File > Open Folder, and select azure-devops-lab).



Day 1, Part 2: Initializing Your First Git Repository
Right now, azure-devops-lab is just a regular folder on your hard drive. Git isn't tracking anything inside it yet.

To tell Git to start monitoring this folder, we need to initialize it as a Git repository.

Hands-on Step 2: Run git init
Inside VS Code, open the built-in terminal by pressing Ctrl + ~ (on Windows/Linux) or Cmd + ~ (on Mac).
(Alternatively, go to the top menu and click Terminal > New Terminal).

Make sure your terminal prompt shows you are inside azure-devops-lab.

Type this command and press Enter:

git init
What Just Happened Behind the Scenes?
Git will print a response like:
Initialized empty Git repository in /your/path/azure-devops-lab/.git/

Git created a hidden folder named .git inside your project directory.

This hidden .git folder is Git's brain.

It stores all your snapshots, configuration, and version history.

Never delete or edit the .git folder manually, or you will lose your repository history!



This concept is the core foundation of how Git works. Once you understand this, Git commands will make total sense instead of feeling like random commands you have to memorize.

The 3 Areas of Git
When you work inside a Git repository, your files travel through three distinct areas:

[ 1. Working Directory ] ──(git add)──> [ 2. Staging Area ] ──(git commit)──> [ 3. Local Repository ]
   (Drafting area)                        (Staging area)                         (Saved snapshot)

1. Working Directory (Unstaged / Untracked)
This is your actual folder where you create, edit, or delete files in VS Code. It's your active workspace. Git sees what you are doing here, but nothing is officially saved into version history yet.

2. Staging Area (Index)
This is a middle holding zone. It allows you to select exactly which files (or specific changes) you want to include in your next saved snapshot.

Why do we need this? Imagine you modified 5 files while working, but only 2 of them are ready to be saved. You put those 2 files into the Staging Area using git add.

3. Local Repository (Committed)
This is Git's permanent storage inside that hidden .git folder we discussed. When you run git commit, Git takes everything currently in the Staging Area, seals it into a single snapshot, and attaches a timestamp, message, and your name/email to it.

Real-World Analogy: Packing a Shipping Box
Think of making a code snapshot like sending a package:

Working Directory: Items sitting scattered on your desk (your unorganized file edits).

Staging Area (git add): Placing specific items inside a cardboard box and leaving it open on your desk. You can add or remove items from the box before closing it.

Local Repository (git commit): Taping the box shut, sticking a shipping label (commit message) on it, and placing it on the warehouse shelf permanently.




Day 1, Part 4: Hands-on with the 3 Areas.

We are going to take a file through all three areas step-by-step using actual commands inside VS Code terminal.

Step 1: Create a New File in Your Working Directory
In VS Code, click the New File icon in the Explorer panel on the left (or go to File > New File).

Name the file: deploy.sh

Type this single line inside deploy.sh:

echo "Deploying Azure Infrastructure..."
Save the file (Ctrl + S on Windows/Linux or Cmd + S on Mac).

Step 2: Check git status (Working Directory)
Open your terminal in VS Code (Ctrl + ~ or Cmd + ~) and run the single most important Git command you will ever use:

git status
What does Git output?
You will see something like this in red text:

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        deploy.sh
What this means:
deploy.sh is sitting in your Working Directory. Git notices that a new file was created, but it is untracked—meaning Git is not keeping track of snapshots for it yet.



Step 3: Move the File to the Staging Area (git add)
In your VS Code terminal, run the following command:

git add deploy.sh
Now, immediately run git status again:

git status
What Changed in the Output?
Notice how the text color turned green, and it now says:


Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   deploy.sh
What this means:
deploy.sh has left the raw Working Directory and is now sitting safely in the Staging Area. You have placed the item into your shipping box!




Step 4: Seal the Snapshot into the Local Repository (git commit)
Now we take everything sitting in the Staging Area and permanently save it as a snapshot in Git's history.

In your VS Code terminal, run this command:

git commit -m "Initial commit: Add Azure deployment script"
Note on -m: The -m flag stands for message. Every commit requires a clear message describing what change was made. In real jobs, writing clear commit messages helps your teammates understand project history.

Step 5: Verify Your Clean State
Now run git status one last time:

git status
What does Git output now?

On branch main
nothing to commit, working tree clean
What this means:
Your package is taped shut, labeled, and safely recorded in the local repository database (.git). There are no unstaged or uncommitted changes left on your desk!


git log

You will see the exact digital record of the commit you just made:

commit 4a8b12c3f9e0... (HEAD -> main)
Author: Your Name <your.email@example.com>
Date:   Sun Jul 26 2026 ...

Initial commit: Add Azure deployment script
What this breakdown means:
Commit Hash (the long string of numbers/letters): The unique ID Git generated for this exact snapshot.

Author: The user.name and user.email you configured earlier.

Date: The exact timestamp when you sealed the snapshot.

Message: Your descriptive message (-m).

Useful Pro-Tip Variant:
In real DevOps projects with hundreds of commits, git log can get very long. Try running this shorter version:

git log --oneline
This prints a clean, single-line summary showing just the short commit ID and your message!

🛑 That's it for today! Take a break.

What you mastered on Day 1:
Created a project folder (azure-devops-lab) and opened it in VS Code.

Initialized a Git repository (git init) and understood what .git does.

Learned the 3 Git areas: Working Directory, Staging Area, and Local Repository.

Created a file, staged it with git add, and created your very first commit with git commit -m.










------------------------------------ DAY 2 -----------------------------------------------------

Before you stage or commit any changes in real DevOps projects, you should always inspect exactly what lines were added, removed, or changed. This prevents accidental hardcoded passwords, typos, or broken logic from sneaking into your commits.

Hands-on Step 3: Run git diff
In your VS Code terminal, run this command:

git diff
What does Git output?
You will see something very similar to this:

Diff
diff --git a/deploy.sh b/deploy.sh
index 1234567..89abcde 100644
--- a/deploy.sh
+++ b/deploy.sh
@@ -1 +1,2 @@
 echo "Deploying Azure Infrastructure..."
+echo "Creating Resource Group: rg-production-eastus"
How to read a git diff:
--- a/deploy.sh (Old version): What the file looked like at your last commit.

+++ b/deploy.sh (New version): What the file looks like right now in your Working Directory.

+ (Green text with plus sign): Lines that were added.

- (Red text with minus sign): Lines that were deleted (if any).

Real-World DevOps Context
Imagine an engineer changes a subnet IP range or an Azure VM size in a script. Running git diff before committing lets you catch high-risk edits instantly before they ever touch production.



Now that you've verified your changes using git diff, you are ready to take these new edits through our 3 Git areas again.

Hands-on Step 4: Stage the Modification
In your VS Code terminal, stage the updated file:

git add deploy.sh
Now, run git diff again:

git diff
Notice Something Interesting?
git diff returned nothing! Why? Because by default, git diff compares your Working Directory to your Staging Area. Since you just staged deploy.sh, there are no unstaged differences left.

Pro-Tip Command: git diff --staged
To see what changes are sitting inside the Staging Area waiting to be committed, run:

git diff --staged
You will see your green added line (+echo "Creating Resource Group: rg-production-eastus") again!

Hands-on Step 5: Commit the Second Snapshot
Now seal this second snapshot into your Local Repository:

git commit -m "feat: Add resource group creation to deployment script"
Hands-on Step 6: Verify Your History with git log
Run your single-line log command:

git log --oneline
What does Git output?
You will now see two snapshots in your timeline, with the newest one at the top:


a1b2c3d (HEAD -> main) feat: Add resource group creation to deployment script
4a8b12c Initial commit: Add Azure deployment script


git diff --- Working Directory vs Staging area
git diff --staged --- Working Area vs Local Repository



In real DevOps work, everyone eventually makes a typo, breaks a script, or stages the wrong file. Git provides simple, safe commands to step backward before those mistakes hit your local history or remote cloud repositories.

Scenario A: Discarding Unstaged Local Changes (git restore)
Imagine you are editing deploy.sh and accidentally type garbage code or delete important logic by mistake, but you have not run git add yet.

Hands-on Step 1: Make a Bad Edit
Open deploy.sh in VS Code.

Add a broken third line at the bottom:


echo "Deploying Azure Infrastructure..."
echo "Creating Resource Group: rg-production-eastus"
echo "BROKEN CODE: delete all resources"
Save the file (Ctrl + S or Cmd + S).

Run git status in your terminal—you'll see red modified: deploy.sh.

Hands-on Step 2: Revert Back using git restore
Instead of manually deleting the bad line in VS Code, let Git throw away your unstaged changes and restore the file to match your last commit:


git restore deploy.sh
What Happened?
Look back at deploy.sh inside VS Code! The broken line is completely gone, and your file is back to its clean state. Run git status—your working tree is clean again!




Scenario B: Unstaging a Staged File (git restore --staged)
Now imagine you made an edit, ran git add deploy.sh, but realized you aren't ready to commit it yet. You want to pull the file back out of the Staging Area (shipping box) back onto your desk without losing your edits.

Hands-on Step 3: Stage an Edit, Then Unstage It
Open deploy.sh in VS Code and add a clean comment at the top:

# Azure Deployment Script
echo "Deploying Azure Infrastructure..."
echo "Creating Resource Group: rg-production-eastus"
Save the file and stage it:

git add deploy.sh
(Run git status—it shows green modified: deploy.sh in the Staging Area).

Now unstage it using the --staged flag:

git restore --staged deploy.sh
What Happened?
Run git status again. Notice the file is now back in red (Changes not staged for commit). Your edits were not deleted—the file was simply moved back from the Staging Area to your Working Directory!


Day 2, Part 5: Ignoring Files with .gitignore
There is one final crucial concept every DevOps engineer uses on Day 2: .gitignore.

In real projects, your directory will fill up with temporary files you never want inside Git—like secrets, API keys, passwords, build logs, OS junk files (.DS_Store, desktop.ini), or terraform state files containing sensitive data.

Instead of accidentally committing them, we list them in a special file named .gitignore.

Hands-on Step 4: Create and Test .gitignore
In your VS Code terminal, let's pretend a log file and a secret file got generated in your folder:


touch app.log secret.env
(If on Windows PowerShell, you can use New-Item app.log, secret.env or just create them via VS Code UI).

Run git status. You will see app.log and secret.env listed in red under Untracked files.

Now, create a new file in your root folder named exactly .gitignore (notice the leading dot!).

Inside .gitignore, type these two patterns:

*.log
*.env
Save .gitignore (Ctrl + S / Cmd + S).

Now run git status again!

What Changed?
app.log and secret.env have completely vanished from git status! Git is now instructed to ignore those files forever. The only untracked file left in red is .gitignore itself.

Let's stage and commit .gitignore so your project keeps these rules permanently:


git add .gitignore
git commit -m "chore: Add .gitignore to exclude logs and env files"


Lets see now about git restore

Scenario 1: Restoring a File to a Specific Past Commit (--source)
Imagine you modified deploy.sh today, but you want to grab the exact version of deploy.sh from your very first commit (Initial commit).

Step 1: Find your past commit ID
Run your single-line log command:

git log --oneline
You'll see something like this:

c3d4e5f (HEAD -> main) chore: Add .gitignore to exclude logs and env files
a1b2c3d feat: Add resource group creation to deployment script
4a8b12c Initial commit: Add Azure deployment script

Step 2: Restore deploy.sh to that first commit (4a8b12c)
Now, tell Git to bring deploy.sh back to how it looked in commit 4a8b12c:

git restore --source=4a8b12c deploy.sh
The Result:
Look at deploy.sh in VS Code! It now only contains your very first line (echo "Deploying Azure Infrastructure..."). The resource group line is gone because Git pulled that exact file state straight out of commit 4a8b12c!

(Run git restore deploy.sh to put it back to normal when you're done testing).