# Local football logos

The project keeps explicitly selected logo files in `logos/`, split into:

- `logos/clubs/` for clubs
- `logos/national-teams/` for national teams
- `logos/competitions/` for competitions

`logos/manifest.json` maps stable source IDs to local file paths. Logo lookup uses the ID and category, never a team name alone. An unmapped or unavailable local logo continues to use the existing provider URL or national-flag fallback.

To add one logo, use a stable ID from the match/standing/competition data and a direct HTTPS image URL from `assets.football-logos.cc`, `images.seeklogo.com`, `media.api-sports.io`, or `cdn.jsdelivr.net`:

```powershell
node scripts/add-logo.js club 12345 https://media.api-sports.io/football/teams/12345.png
node scripts/add-logo.js national-team 777 https://media.api-sports.io/football/teams/777.png
node scripts/add-logo.js competition 39 https://media.api-sports.io/football/leagues/39.png
```

The importer downloads only the requested file, checks the response and image signature, limits downloads to 5 MB, rejects unsafe SVG content, and refuses to overwrite an existing ID or file. It updates the manifest only after validating the image. No image is downloaded automatically during page loads. Restart the local server, or redeploy on Render, after adding a mapping so the server reloads the manifest.

Local images are served from `/logos/<category>/<id>.<extension>` with browser caching. They are deployed with the project files on Render; add both the image and the corresponding manifest entry to the deployment source. Use PNG, JPEG, WebP, or a self-contained SVG, and retain the source image's dimensions and aspect ratio.
