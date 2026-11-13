# Brooke's homework hub

A single-file homework site for Brooke, a 6th grader. Open the site, pick Math or Social Studies, and work through the assignment in the browser.

Answers and written work stay in that browser. Completed checkmarks are saved in that browser too, and they can sync between devices once a Firebase database URL is pasted in (see below). New assignments get added to `index.html`.

Pushes to `main` publish this folder with GitHub Pages.

## Sync completed checkmarks

The page never stores a password or a private token. Completion is a true/false flag for each assignment id, plus a timestamp. Brooke's name and her answers are not sent anywhere.

Until the URL below is filled in, completed checkmarks stay on the device she is using.

1. Go to [Firebase console](https://console.firebase.google.com) and create a free project.
2. Build the project, then create a **Realtime Database**. You can start it in locked mode. Any region is fine.
3. Open the **Rules** tab and replace the rules with:

```json
{
  "rules": {
    "completed": {
      ".read": true,
      "$id": {
        ".write": "$id.matches(/^[a-z0-9-]{1,40}$/) && (!data.exists() || newData.child('updatedAt').val() >= data.child('updatedAt').val())",
        ".validate": "newData.hasChildren(['value', 'updatedAt']) && newData.child('value').isBoolean() && newData.child('updatedAt').isNumber() && newData.child('updatedAt').val() > 0 && newData.child('updatedAt').val() <= now + 86400000"
      }
    }
  }
}
```

4. Copy the database URL. It looks like `https://your-project-default-rtdb.firebaseio.com` or `https://your-project-default-rtdb.region.firebasedatabase.app`.
5. In `index.html`, search for `firebaseDatabaseUrl` and paste that URL between the quotes.
6. Commit and push to `main` so GitHub Pages publishes it.

Anyone who can view the website can see that URL and could flip a completed flag. They cannot see Brooke's name or her written answers. Those stay on each device.
