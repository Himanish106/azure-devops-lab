Lets imagine a scenario:

You are an IT professional working on a feature or configuration.You are in the middle of writing complex Azure infrastructure code. Suddenly, there is a production outage that requires a quick fix on the main branch.

You cannot git switch main because your uncommitted changes in deploy.sh would be overwritten or cause conflicts. You also cannot git commit because your code is broken and unfinished.

git stash solves this by taking your uncommitted changes (both staged and unstaged) and moving them into a safe local storage area leaving your directory clean so you can switch branches.