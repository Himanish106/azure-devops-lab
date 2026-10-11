Azure Boards: Azure Boards is basically the digital to-do list and project manager for your engineering team (similar to Jira or Trello). Like it is a project management tool that is used to track the progress of your project like how it is going who is working on what tasks and all the things that we can improve in further releases.

Work Item: Work Item is nothing but something through which we can track activity on our project. It can be a bug, a feature a user story, epic etc. It is assigned to some person and the person works on it.


Why do we need Azure Boards?

In a real company:

a) You never write code randomly.

b) Every change starts with an assigned task (e.g., "Create a Resource Group in East US").

c) Azure Boards tracks what needs to be done, who is doing it, and when it is finished.

The Big Picture: How Boards Connect to Git.

Instead of updating your manager manually when you finish writing the code, Azure Devops connect your board task to your Git code automatically.

[Task #1 Created in Boards] 
            │
            ▼
[You create branch: feature/infra-baseline]
            │
            ▼
[You write code & commit: "feat: Added RG #1"]
            │
            ▼
[You open Pull Request & Merge to main]
            │
            ▼
[Task #1 automatically moves to "Done" in Boards!]


Some Key Terms Explained:

a) Epic: A massive multi month project (e.g. "Migrate entire company to Azure")
b) Feature: A smaller piece of the project (e.g. Setup Azure networking)
c) User Story / Task: A specific job for one engineer. (e.g. "Write terraform code to provision a resource group")
d) Tracebility: Being able to look at any running azure resource or code commit and immediately see the exact task ticket and the engineer responsible for it.


When creating an Azure DevOps project, the Work Item Process dictates the terminology, workflow states, and rules that Azure Boards uses to track work.

Azure DevOps provides 4 standard built-in processes:

a) Basic(Simplest):

i) What it is: A lightweight board setup with minimal fields and no formal methodology.
ii) Work Item Hierarchy: Epic ----> Issue ----> Task
iii) States: To Do, Doing, Done
iv) When to Pick it: When you don't need formal agile methodology like "User Stories" or "Story Points" and just want a simple Kanban Board.


b) Agile (Most common in Cloud / Devops)

i) What it is: Uses standard Agile terminology. It plans work around what the customer or system needs.
ii) Work Item Hierarchy: Epic ----> Feature ----> User Story -----> Task.
iii) States:

New: Ticket logged.

Active: Work started.

Resolved: Code written, tested, or waiting for deployment.

Closed: Fully merged and delivered.

iv) Key Feature: Tracks bugs either alongside User Stories or as separate tasks.


c) Scrum (Sprint and Backlog Centric): 

i) What it is: Strictly models the scrum guide
ii) Work Item Hierarchy: Epic -----> Feature -----> Product Backlog Item (PBI) -----> Task
iii) Differences from agile

Uses PBI (Product Backlog Item) instead of "User Story".

Tracks an Impediment (obstacle) work item for blockers.

States represent sprint planning stages: New -----> Approved -----> Committed (team committed for this sprint) -----> Done.

d) CMMI (Capability Maturity Model Integration): 

i) What it is: Designed for heavily regulated enterprises where every change requires formal approvals, change requests, and risk assessments.

ii) Work Item Hierarchy: Epic -----> Feature -----> Requirement -----> Task.

iii) Special Items: Explicit tracking for Change Requests, Risks, and Reviews.

iv) When to pick it: Enterprise defense, government, or healthcare contracts where auditors need detailed justification for every change.
