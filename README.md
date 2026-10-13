# Every new person adds more than one conversation

A live animation that shows how communication effort grows as a team gets bigger.

- **Flat team:** everyone talks to everyone, so channels grow as n(n−1)/2.
- **Squads with leads:** a head, squad leads and members. Far fewer channels, but messages need up to 3 hops to cross the team.

It's a single static `index.html` (vanilla JS + canvas, no build step, no dependencies besides Google Fonts).

## Publish on GitHub Pages

1. Create a new **public** repository on GitHub (for example `team-communication`).
2. Upload the files in this folder (`index.html`, `README.md`, `.nojekyll`) to the root of the `main` branch.
   - Web: **Add file → Upload files**, drag them in, commit.
   - Or from a terminal:
     ```bash
     git init
     git add .
     git commit -m "Team communication animation"
     git branch -M main
     git remote add origin https://github.com/<your-user>/team-communication.git
     git push -u origin main
     ```
3. In the repo go to **Settings → Pages**, set **Source** to *Deploy from a branch*, choose `main` and `/ (root)`, and save.
4. After a minute the site is live at `https://<your-user>.github.io/team-communication/`.

To update it, edit `index.html` and push; Pages redeploys automatically.

## Run locally

Just open `index.html` in a browser, or serve the folder:

```bash
python3 -m http.server 8000
```
