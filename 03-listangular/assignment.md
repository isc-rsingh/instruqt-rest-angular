---
slug: listangular
id: y8oxk6vrcu5p
type: challenge
title: Add List service to Angular
teaser: Fetching data with REST service on the Angular side
notes:
- type: text
  contents: |-
    Now we have a REST service to access the data. Let's add it to the front-end UI!

    In today's sample environment, we have a very simple Angular app built with the Angular Material library. At the moment it doesn't use any REST services at all, and only relies on sample data built into the app itself. Let's make it more interactive!
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
- title: SQL
  type: service
  hostname: iris
  path: /csp/sys/exp/%25CSP.UI.Portal.SQL.Home.zen?$NAMESPACE=USER
  port: 52773
difficulty: basic
timelimit: 900
---
On the left in the VSCode tab, expand the `angular/src/app` folder and all of it's child folders.

Here you see a simple web application with a list provided by the `BookmarkListComponent` Angular class, and a form provided by the `BookmarkFormComponent` Angular class. At the moment all data is coming from the application itself.

Let's go to the "VSCode" tab, open `angular/src/app/bookmark-list/bookmark-list.component.ts` file and in the `ngOnInit` method replace the following line:

```JS
this.dataSource=of(SAMPLE_DATA);
```

With this one:

```JS
this.dataSource =
  this.http.get(environment.API_URL+'/list');
```

This will call our `list` API endpoint and get the list of bookmarks into table.

Save the file and switch to the "APP" tab, you will see an updated table!

Now, let's switch to the "SQL" tab and add another record into our IRIS table:

```SQL
INSERT INTO Sample.Bookmark
  (Url, Description, DateAdded, TimeAdded)
  VALUES ('https://community.intersystems.com',
          'InterSystems Developers Community',
          CURRENT_DATE,CURRENT_TIME)
```

Switch back to the "APP" tab, click "Refresh" button in the header, and you should see a table updated.
