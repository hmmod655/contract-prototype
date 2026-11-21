# Contract review prototype: hosting

`index.html` is the whole prototype in one file. It needs no server code and no database.

Reviewer notes are kept in two places: in the reviewer's browser (localStorage) as soon as they
save a note, and inside the HTML file itself when they press «حفظ الملف مع الملاحظات». That button
saves a copy of the page with every note embedded; the reviewer sends that file back. Opening a
received file loads its notes and merges them with the notes already in that browser, so opening
each reviewer's file in turn collects everyone's notes in one place. Save again to get one combined file.

## Azure Static Web Apps (recommended): Reef sign-in, invited reviewers only

`staticwebapp.config.json` sends anyone not signed in to Microsoft sign-in and only lets
in people you have invited with the role `reviewer`. Everyone else sees `403.html`.

1. Azure portal → Create a resource → Static Web App. Plan: Free. Deployment source: **Other**.
2. Open the new app → Overview → **Manage deployment token**, copy it.
3. From the folder that contains this README, run (Node.js required):

       npx @azure/static-web-apps-cli deploy . --deployment-token <TOKEN> --env production

4. App → **Role management** → **Invite**: provider *Microsoft Entra ID*, the reviewer's
   `@reef.sa` email, role `reviewer`. Send them the invitation link it produces.
5. To update the prototype later, replace `index.html` and run step 3 again.
   Reviewers' notes survive updates as long as the site address stays the same.

## Anywhere else

Any static host serves `index.html` as is (IIS, nginx, Azure Blob static website, Netlify).
Without the Azure config file there is no sign-in, so anyone with the link can open it.
