---
slug: rest
id: ce3jzl4vdnxy
type: challenge
title: REST Service
teaser: Accessing your data
notes:
- type: text
  contents: |-
    In this exercise, you will build a very simple single-page URL bookmarking application.
    
    It will allow you to save URLs into the database and then retrieve and display them in an attractive list format in a web app.
- type: text
  contents: |-
    The only server-side technology we need to accomplish this is the InterSystems IRIS data platform.

    It serves as data layer, application server, and more!

    It provides a single, integrated data layer that can be accessed using one of several data models. Within InterSystems IRIS, data can be modeled and stored as tables, objects, multidimensional arrays, or semi-structured documents. Different data models can seamlessly access the same data without the need for performance-killing mapping between models, and with full concurrency!

    In today's exercise we'll use SQL and Object models to access the same data, depending on task at hand.
- type: text
  contents: |-
    InterSystems IRIS also gives you many options to work with the data, from built-in analytics and interoperability solutions to interfaces with external languages and systems. One of the options is creating REST services using a built-in programming language ObjectScript, which we will explore today.

    Please wait for the training environment to initialise and press Start button when ready
tabs:
- title: IRIS SQL Explorer
  type: service
  hostname: iris
  path: /csp/sys/exp/%25CSP.UI.Portal.SQL.Home.zen?$NAMESPACE=USER
  port: 52773
- title: IRIS
  type: terminal
  hostname: iris
difficulty: basic
timelimit: 900
---
First of all, let's create a data storage structure in InterSystems IRIS to hold our application's data. There are multiple ways to do this. We start by creating a persistent ObjectScript Class, or a SQL table. But the truth is, in IRIS you will end up with both anyway! Let's use the classic SQL data model approach this time, because it's really fast and simple.

Copy and Paste the following SQL CREATE statement in the SQL "Execute Query" box on the left (below the "Execute" button) to create the <code>Sample.Bookmark</code> table.

Then press the "Execute" button to run the query.

```SQL
CREATE TABLE Sample.Bookmark
(
  Url VARCHAR(1000),
  Description VARCHAR(1000),
  DateAdded DATE,
  TimeAdded TIME
)
```

Now let's add some data into the new table. You can use our sample below or change it to bookmark some other site you like. You can also add multiple bookmarks by changing the values and running the SQL statement again.

Execute the following SQL statement:

```SQL
INSERT INTO Sample.Bookmark
(Url, Description, DateAdded, TimeAdded)
VALUES ('https://intersystems.com','InterSystems',CURRENT_DATE,CURRENT_TIME)
```
