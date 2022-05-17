---
slug: addangular
id: ob4rol66sx14
type: challenge
title: Add service in Angular
teaser: Adding data with REST service on the Angular side
notes:
- type: text
  contents: 'The final bit: implementing the Add button in the user interface!'
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
Let's finish hooking up our Angular UI into freshly built InterSystems IRIS REST API.

Let's go to the "VSCode" tab, open the `angular/src/app/bookmark-list/bookmark-form.component.ts` file, and replace an empty `addBookmark` method with the following content:

```JS
  addBookmark(): void {
    this.http.post(environment.API_URL+'/add',{url:this.url, description:this.description}).pipe(
      switchMap(()=>this.snackBar.open('New URL added', 'OK').afterDismissed()),
      tap(()=>window.location.reload()),
      catchError(()=>this.snackBar.open('Something went wrong', 'OK').afterDismissed()),
    ).subscribe()
  }
```

In the first line of this method we are calling our `/add` endpoint, sending the new URL and a description. Then we process the result by either showing a success message and reloading the window, or showing an error message if there was an error.

Add a couple of bookmarks to the database and see your table grow!