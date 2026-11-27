# Brooke's homework hub

A homework site for Brooke, a 6th grader. It keeps three things: the South America practice test, Math Unit 4 practice for ratios and rates, and the My Grades page.

Answers and written work stay in that browser. Finished scores, and whether a practice is completed, sync between devices through Firebase. New assignments get added to `index.html`.

Social Studies also includes the South America Review Game. It is a separate question set from the South America Practice Test, with its own id, `sa-review-game`.

Pushes to `main` publish this folder with GitHub Pages.

## Scores and completed practices

A practice moves into the Completed section when every question is finished and the score is at least 70%. That cutoff is `PASS_PERCENT` in `index.html`. Each question has two scored tries. A correct answer on the first or the second try counts the same. The choices are shuffled, and they lock after Check.

A wrong first try shows a hint that explains the method and does not give the final answer. She then waits 2 minutes, with the end time saved in this browser and in Firebase, before the second try unlocks. A second miss is saved as wrong and the solution appears. After the practice is finished, Retry missed questions is practice only and does not change the saved score.

There is no Mark as completed button.

The page never stores a password or a private token. Each practice is one record at `/hub/v1/items/<itemId>` with `completed`, `completedAt`, `attempts`, `lastScore`, `bestScore`, and `questions`. A question result is `first`, `second`, `wrong`, or `wait` while the 2 minute lockout is running (`lockoutUntil`). A score is the number correct, the total, a percent, and a timestamp. Her name and her written answers are not sent.

The database URL is already filled in at `firebaseDatabaseUrl` in `index.html`. Paste `database.rules.json` into the Realtime Database rules tab in the Firebase console and publish it. Until those rules are published, the database stays in test mode. Anything outside `/hub/v1/items/$itemId` is denied, and each record has to match the fields above.

Anyone who can view the website can see that URL and could change a score. They cannot see her name or her written answers. Those stay on each device. If the network fails, the page keeps working from this browser and tries again on its own.
