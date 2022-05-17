---
slug: listiris
id: xnff7mowf7wg
type: challenge
title: List REST service
teaser: Fetching data with REST service using IRIS ObjectScript
notes:
- type: text
  contents: |-
    Now that we have data stored, let's build a REST service to work with it.

    In InterSystems IRIS, a REST service is represented by a _non-persistent_ class, which has a Routing section to connect incoming URL requests to handler methods, and the handler methods themselves.

    In today's sample environment, we have an empty REST-enabled ObjectScript class, `Sample.RestService`, set up for you.
tabs:
- title: VSCode
  type: service
  hostname: vscode
  path: /?workspace=/opt/intersystems/backend.code-workspace
  port: 8080
- title: SQL
  type: service
  hostname: iris
  path: /csp/sys/exp/%25CSP.UI.Portal.SQL.Home.zen?$NAMESPACE=USER
  port: 52773
difficulty: basic
timelimit: 900
---
On the left, click on `RestService.cls` to open the file for editing.

Here you see a REST-enabled ObjectScript class opened in VSCode with a single test route defined. Have a look and see how the code is organised.

Now, let's add another route which will return a list of all bookmarks we have in our database. First, add the following route into the UrlMap section:

```
  <Route Url="/list" Method="GET" Call="List" />
```

This will call the `List()` method of our class when a request to the `/list` API endpoint is received. Let's add the List metod as well:

```
ClassMethod List() As %Status
{
  set list=[],sc=1
  set sql="SELECT Url, Description, DateAdded, TimeAdded FROM Sample.Bookmark"
  set Statement = ##class(%SQL.Statement).%New()
  set sc=$$$ADDSC(sc,Statement.%Prepare(sql))
  set Result = Statement.%Execute()
  if (Result.%SQLCODE<0) {
    set sc=$$$ADDSC(sc,$$$ERROR(5001, "Error executing sql statement"))
  }
  while Result.%Next() {
    set bookmark={
      "url":(Result.Url),
      "description":(Result.Description),
      "dateAdded":($zd(Result.DateAdded,3)),
      "timeAdded":($zt(Result.TimeAdded,2))
    }
    do list.%Push(bookmark)
  }
  if $$$ISOK(sc) write list.%ToJSON()
  return sc
}
```

Save this class and go to the next assignment.