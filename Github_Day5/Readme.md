Part 1: What is a Pull Request?

In an enterprise environment, developers never push directly to main. Direct pushes bypasses tests and code audits, increasing the risk of breaking production infrastructure. So the work flow is like this:

a) Make changes on your development branch (dev).
b) Push dev to Github
c) Open a Pull Request (PR) asking permission to merge dev into main
d) Peer engineers review the difference, comment on lines of code, and approve it.
e) Automated CI/CD pipelines runs the test.
f) The peer is merged directly on Github.

Local Machine:    [dev] ──(git push)──> GitHub: [origin/dev]
                                                    │
                                          [Open Pull Request]
                                                    │
                                            [Peer Code Review]
                                                    │
                                            [Merge into main]
                                                    │
Local Machine:    [main] <──(git pull)── GitHub: [origin/main]


Part 2: Hands-on: Create and Merge a Pull Request