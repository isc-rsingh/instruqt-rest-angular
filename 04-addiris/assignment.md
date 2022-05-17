---
slug: addiris
id: fcvkxtyebnmd
type: challenge
title: Add REST service
teaser: Sending data with REST service using IRIS ObjectScript
notes:
- type: text
  contents: |-
    Now that we can retrieve bookmarks from InterSystems IRIS and display it in the app. Let's implement an ability to add bookmarks as well.

    So far in this exercise we used SQL access to manupulate data. Let's now use object access to add new records into our InterSystems IRIS database.
tabs:
- title: VSCode
  type: service
  hostname: vscode
  path: /?workspace=/opt/intersystems/instruqt-rest-angular.code-workspace
  port: 8080
- title: APP
  type: service
  hostname: vscode
  path: /
  port: 4200
- title: IRIS
  type: service
  hostname: iris
  path: /csp/sys/exp/%25CSP.UI.Portal.SQL.Home.zen?$NAMESPACE=USER
  port: 52773
difficulty: basic
timelimit: 900
---
Similarly to what we did before when implementing list functionality, create functionality will need a new API endpoint.

In the "VSCode" tab, open `iris/src/Sample/RestService.cls` and add the following route into UrlMap section:

```XML
  <Route Url="/add" Method="POST" Call="Add" />
```

Note that here we use *HTTP POST* method, which is a best practice in REST architecture to create new data.

Now add the method implementation:

```
/// Add one bookmark to database
ClassMethod Add() As %Status
{
  set sc=$$$OK
  set jsonRequest=##class(%Library.DynamicObject).%FromJSON(%request.Content)
  set Bookmark=##class(Sample.Bookmark).%New()
  set Bookmark.Url=jsonRequest.url
  set Bookmark.Description=jsonRequest.description
  set Bookmark.DateAdded=$piece($horolog,",",1)
  set Bookmark.TimeAdded=$piece($horolog,",",2)
  set sc=$$$ADDSC(sc,Bookmark.%Save())
  return sc
}
```

Save this class and go to the next assignment.