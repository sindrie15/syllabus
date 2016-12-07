## Objectives
* Continue implementing the game logic test driven
* Get a test report displayed in Jenkins for tests run.

## Steps
* Get result of test into a xml file
* Use the post-build option in Jenkins to publish unit test result report.
Hint:
You can use [this project](https://github.com/larrymyers/jasmine-reporters) to get test result from Jasmin

* For each scenario written in day 7 exercise, write a corresponding test.
* Take care to respect casing of existing attributes, such as gameId, and phrasing of commands/events. Some are already in
use in client. If in doubt, do a global search in project.
* Be very mindful of committing your changes when each test has passed for the first time. We expect you to write this
code yourself, no copy-pasting code from others in this execise. You can look and learn from others, but write the code
yourself.
* There is a missing column in th evenlog table. Correct this by adding a migration. Changing an existing migration is
 the wrong way of doing this.
Hint:
    [db migrate docs for adding column](https://db-migrate.readthedocs.io/en/latest/API/SQL/#addcolumntablename-columnname-columnspec-callback)


## State (when are you done)

* You will get a Test Result link in Jenkins for each build where you can see visual representation of the test results.
* Have a working game server side.
* Have the server working again.
* You have implemented passing tests for all cases written in day 7


## PS

We fully expect this to take a couple of days.