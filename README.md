# Introduction to Query Plan

## Introduction

Anyone experienced with SQL servers probably ran into some issues with query performance. Query plan helps you to understand how the computation is running, and if you know how to use this tool, you can see how you can optimize queries for performance.

## Objectives
- Learn how to use query plan tool


### What is query plan?

A query plan is a list of instructions that the database follows in order to execute a query on the data. Query plans show the steps taken to execute the given command. Depending on the database you use, it can also specify the expected cost for each section of the command. The queries are transformed into executable commands by query optimizier. The Query Optimizer generates multiple Query Plans for a single query and determines the most efficient plan to run.
Depending on the GUI tool you use, there may be a query planner tab, but from the cli tools, you can get the same effect using `EXPLAIN` or `EXPLAIN QUERY PLAN`.

### How to use it?

In this exercise, we're going to have a glimpse of how to view query plan.

Open a terminal window, and enter `sqlite` to start sqlite cli.

Enter following DDL below:

```
CREATE TABLE contacts (
	first_name text NOT NULL,
	last_name text NOT NULL,
	email text NOT NULL
);
```

We're also going to create an index, that we'll talk about later.

```
CREATE UNIQUE INDEX idx_contacts_email 
ON contacts (email);
```

Then insert some records:

```
INSERT INTO contacts (first_name, last_name, email)
VALUES('David','Brown','david.brown@flatironschool.com'),
      ('Lisa','Smith','lisa.smith@flatironschool.com');
```      

At this point, you should be able to run this query:

```
SELECT
	first_name,
	last_name,
	email
FROM
	contacts
WHERE
	email = 'lisa.smith@flatironschool.com';
```

You should see something like this.

!(query_result)[./images/query_result.png]

The next step, we'll use the `EXPLAIN QUERY PLAN` keyword to see what it shows.

```
EXPLAIN QUERY PLAN 
SELECT
      first_name,
      last_name,
      email
FROM
      contacts
WHERE
      email = 'lisa.smith@flatironschool.com';
```

You'll see as the result, the query plan shows:

```
QUERY PLAN
`--SEARCH TABLE contacts USING INDEX idx_contacts_email (email=?)
```

It's a fairly simple exercise so the query plan is very simple. It tells exactly the query executed by searching a table named `contacts` using index that we created earlier.

As we progress, we'll use the query plan in much more complicated queries to see what really goes behind SQL servers.

### Summary

In this lesson, we went over briefly what query plan is and how to view query plan through cli tools. If there are GUI tools available, you may be able to see more friendly version of query plans.
