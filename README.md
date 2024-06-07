[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-24ddc0f5d75046c5622901739e7c5dd533143b0c8e959d652212380cedb1ea36.svg)](https://classroom.github.com/a/gBpPdvVg)
# Fact Data Modeling

## Submission Guidelines

To ensure smooth processing of your submissions through GitHub Classroom automation, we cannot accommodate individual requests for changes. Therefore, please read all instructions carefully.

1. **Sync fork:** Ensure your fork is up-to-date with the upstream repository. You can do this through the GitHub UI or by running the following commands in your local forked repository:

    ```bash
    git pull upstream main --rebase
    git push origin <main/your_homework_repo> -f
    ```

3. **Open a PR in the upstream repository to submit your work:**
    - Open a Pull Request (PR) to merge changes from your `main` or custom `homework` branch in your forked repository into the main branch in the upstream repository, i.e., the repository your fork was created from.
    - Ensure your PR is opened correctly to trigger the GitHub workflow. Submitting to the wrong branch or repository will cause the GitHub action to fail.
    - See the [Steps to open a PR](#steps-to-open-a-pr) section below if you need help with this.

5. **Complete assignment prompts:** Write your SQL in the query file corresponding to the prompt number in the **`submission`** folder. Do not change or rename these files!
    >
    > :warning: The `_app` folder, `Dockerfile`, and `.github` folder are crucial for automated tests and feedback.
Do not modify these files/folders. Focus only on the files within the submission folder.
    >

6. **Lint your SQL code for readability.** Ensure your code is clean and easy to follow.

7. **Add comments to your queries.** Use the **`--`** syntax to explain each step and help the reviewer understand your thought process. 

A link to your PR will be automatically shared with our TA team. They will review and grade submissions after the homework deadline.

### Steps to open a PR
  1. Go to the upstream [**`fact-data-modeling-pub`**](https://github.com/DataExpert-ZachWilson-V4/fact-data-modeling-pub) repository
  2. Click the [**Pull Requests**](https://github.com/DataExpert-ZachWilson-V4/fact-data-modeling-pub/pulls) tab.
  3. Click the **"New pull request"** button on the top-right. This will take you to the [**"Compare changes"**](https://github.com/DataExpert-ZachWilson-V4/fact-data-modeling-pub/compare) page.
  4. Click the **"compare across forks"** link in the text.
  5. Leave the base repository as is. For the **"head repository"**, select your forked repository and then the name of the branch you want to compare from your forked repo.
  6. Click the **"Create pull request"** button to open the PR

**:warning: :exclamation: IMPORTANT CONSIDERATIONS & REMINDERS :exclamation: :warning:**
  - Do not close or merge your PR, as this will affect the codebase for others and make it difficult for TAs to find your submission.
  - You can revise and push changes to the PR before the deadline, but avoid making changes after the deadline, as they will not be reviewed and may cause confusion.
  - Some assignments include tests or feedback generated by GitHub Actions, which you can use to refine your solutions before the deadline.
  - Enhance your review by adding comments under 'Files changed' to summarize or highlight key parts of your code. If you've already added comments within your code, you can reiterate or summarize those comments here.

### Grading
  - Grades are pass or fail, used solely for certification.
  - Final grades will be submitted by a TA **after the deadline**.
  - An approved PR means a Pass grade. If changes are requested, the grade will be marked as Fail.
  - Reviewers may provide comments or suggestions with requested changes. These are optional and intended for your benefit. Any changes you make in response will not be re-reviewed.

Assignment
==================

## De-dupe Query (`query_1.sql`)

Write a query to de-duplicate the `nba_game_details` table from the day 1 lab of the fact modeling week 2 so there are no duplicate values.

You should de-dupe based on the combination of `game_id`, `team_id` and `player_id`, since a player cannot have more than 1 entry per game.

Feel free to take the first value here.

Note: For this query, you need to filter out duplicate records and display only unique records. To filter out duplicate records, you can use the combination of game_id, team_id, and player_id columns. You can achieve this either by using ROW_NUMBER() or by using GROUP BY.

## User Devices Activity Datelist DDL (`query_2.sql`)

Similarly to what was done in day 2 of the fact data modeling week, write a DDL statement to create a cumulating user activity table by device.

This table will be the result of joining the `devices` table onto the `web_events` table, so that you can get both the `user_id` and the `browser_type`.

The name of this table should be `user_devices_cumulated`.

The schema of this table should look like:

- `user_id bigint`
- `browser_type varchar`
- `dates_active array(date)`
- `date date`

The `dates_active` array should be a datelist implementation that tracks how many times a user has been active with a given `browser_type`.

Note that you **can** also do this using a `MAP(VARCHAR, ARRAY(DATE))` type, but then you have to know how to manipulate the contents of those maps correctly (and then you don't include a `browser_type` column).
If you use the `MAP` type, you'd have one row per `user_id`, and the keys of this `MAP` would be the values for `browser_type`, and the values would be the arrays of dates for which we saw activity for that user on that browser type.

Note only that, but you'll need to take care of doing the CROSS JOIN UNNEST correctly - when we did it in lab, we didn't do it against a `MAP` type, but an `ARRAY` type, so it exploded into rows in the way you'd expect.

Doing this by just including a `browser_type` column means it works almost exactly the same as what we did in lab, you just add an additional group by key.

The first index of the date list array should correspond to the most **recent** date (today's date).

## User Devices Activity Datelist Implementation (`query_3.sql`)

Write the incremental query to populate the table you wrote the DDL for in the above question from the `web_events` and `devices` tables. This should look like the query to generate the cumulation table from the fact modeling day 2 lab.

## User Devices Activity **Int** Datelist Implementation (`query_4.sql`)

Building on top of the previous question, convert the date list implementation into the base-2 integer datelist representation as shown in the fact data modeling day 2 lab.

Assume that you have access to a table called `user_devices_cumulated` with the output of the above query. To check your work, you can either load the data from your previous query (or the lab) into a `user_devices_cumulated` table, or you can generate the `user_devices_cumulated` table as a CTE in this query.

You can write this query in a single step, but note the three main transformations for this to work:

- unnest the dates, and convert them into powers of 2
- sum those powers of 2 in a group by on `user_id` and `browser_type`
- convert the sum to base 2

## Host Activity Datelist DDL (`query_5.sql`)

Write a DDL statement to create a `hosts_cumulated` table, as shown in the fact data modeling day 2 lab. Except for in the homework, you'll be doing it by `host`, not `user_id`.

The schema for this table should include:

- `host varchar`
- `host_activity_datelist array(date)`
- `date date`

## Host Activity Datelist Implementation (`query_6.sql`)

As shown in the fact data modeling day 2 lab, Write a query to incrementally populate the `hosts_cumulated` table from the `web_events` table.

## Reduced Host Fact Array DDL (`query_7.sql`)

As shown in the fact data modeling day 3 lab, write a DDL statement to create a monthly `host_activity_reduced` table, containing the following fields:

- `host varchar`
- `metric_name varchar`
- `metric_array array(integer)`
- `month_start varchar`

## Reduced Host Fact Array Implementation (`query_8.sql`)

As shown in fact data modeling day 3 lab, write a query to incrementally populate the `host_activity_reduced` table from a `daily_web_metrics` table. Assume `daily_web_metrics` exists in your query. Don't worry about handling the overwrites or deletes for overlapping data.

Remember to leverage a full outer join, and to properly handle imputing empty values in the array for windows where a host gets a visit in the middle of the array time window.

Note: For this query, you will use the daily_web_metrics table, which you created in Week 2, Lab 3. In Lab 3, you created this daily_web_metrics table with a user column but without a host column. For this query, you need to re-create the table with the host column and populate data into the daily_web_metrics table. Ensure that this table is created in your schema, as you did in Week 2 Lab. Mention the schema name in your CTE/query during the submission.