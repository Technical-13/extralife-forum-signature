# extralife-forum-signature
Makes Extra Life fundraising progress bars as plain PNG images, so you can use them in a forum signature (like XenForo) where only an image link will work.

GitHub checks your totals every 6 minutes, redraws the images, and publishes them with GitHub Pages. You end up with a permanent image URL that always shows your current progress.

## What you get
| File | What it shows |
| --- | --- |
| `participant-bar.png` | Your own total and goal |
| `team-share-bar.png` | The team's goal, split into your share (blue), everyone else's share (yellow), and what's left (empty) |
| `team-bar.png` | Your team's total and goal |

If you don't have a team, you only get `participant-bar.png`.

## How to use it for your own page
### 1. Fork this repo
Click **Fork** at the top of this page. You can keep the same name or pick your own.

**IDs are different every event year. If an ID from last year gives a 404 error, you need this year's ID.**

### 2. Configure `dd-extralife-configs.json` for your campaign.
Open `dd-extralife-configs.json` in your fork and click the pencil icon to edit it:
```json
{
  "apiRunsToKeep": 21,
  "discordNotifyOnError": true,
  "eventID": 562,
  "lang": "en-US",
  "participantID": 123456,
  "teamID": null
}
```

- **`apiRunsToKeep`** How many runs worth of DonorDrive Extra Life API query data to hang on to for debugging/logging history.  **MAX: 25**
- **`discordNotifyOnError`** Boolean toggle for if the process should ping you in a new DM on Discord if it encounters an error.
- **`eventID`** The script uses this to validate if your ID belongs to the current year.  **2026 event id is `562`**
  - Previous events: `{ "2025": 559 }`
- **`lang`** Currency/Date/Time string formatting language.
- **`participantID`** Your participant ID is the `123456` on the end of your default fundraising link such as `dd.extra-life.org/participants/123456`.
  - If you've customized your link, you can also find your participant ID in the settings section of your Extra Life fundraising page.
    - Just replace `CUSTOMIZED_LINK` with your actual customized name in `dd.extra-life.org/participants/CUSTOMIZED_LINK#settings`
  - Set `participantID` to `null,` if you want only a team bar.
- **`teamID`** If you are on a team, the script attempts to find this for you.
  - You may set this to explicitly override auto-detection if it is wrong or fails to load.
  - You may set this and explicitly set `"participantID": null,` if you want only your bar to be created.
- You need at least one of `participantID` or `teamID` for this to run.

Commit the change.

### 3. Turn on Actions
In your fork, open the **Actions** tab. GitHub turns off scheduled jobs on forks, so click the green button to enable workflows.

### 4. Turn on GitHub Pages
Go to **Settings > Pages**. Under "Build and deployment," pick **Deploy from a branch**, choose `main` and `/ (root)`, then Save.

### 5. Run it once
Go to **Actions**, pick **Update bar** on the left, and click **Run workflow**. Wait for the green check. After that it updates by itself every 30 minutes.

If the run fails, open it and read the red error. The most common causes are a wrong ID, or an ID from a different year.

### 6. Use the image
After a minute or two, your images are here:
```
https://YOUR-USERNAME.github.io/YOUR-REPO-NAME/participant-bar.png
https://YOUR-USERNAME.github.io/YOUR-REPO-NAME/team-bar.png
https://YOUR-USERNAME.github.io/YOUR-REPO-NAME/team-share-bar.png
```

Open one in your browser to check it. Then in a XenForo signature (or any forum that uses BBCode), add:
```
[IMG]https://YOUR-USERNAME.github.io/YOUR-REPO-NAME/participant-bar.png[/IMG]
```

## Things to know
- **Updates are not instant.** GitHub Pages can take several minutes to show a new image after each update.
- **Your forum may keep an old copy.** Many forums keep their own cached copy of images in signatures. If your bar looks stuck, that is the likely reason, and only the forum admin can change it.
- **Scheduled runs can be late or skipped.** GitHub does not promise exact timing. Use **Run workflow** if you want an update right now.
- **Inactive repos get paused.** GitHub may pause scheduled jobs in a repo that has had no activity for a long time. If your bar stops updating, check the Actions tab.
- **If the bot cannot save the images,** go to **Settings > Actions > General > Workflow permissions** and choose **Read and write permissions**.

## How it works
- `dd-extralife-configs.json` holds your customized configuration settings (including your IDs). This is the only file most people need to edit.
- ~~`TBD` reads your numbers from the Extra Life API at `dd.extra-life.org/api` and draws the PNG files.~~
- ~~`.github/workflows/update.yml` runs the script every 6 minutes and saves the new images to the repo.~~

You can also run it on your own computer with Node.js 24 LTS or newer:
```
npm install
node 
```

## Fonts and colors
The bars use Open Sans and the Children's Miracle Network brand colors (CMN Blue, CMN Yellow, and CMN Dark Gray). Open Sans is the free web font listed in the CMN brand guidelines for cases where their main fonts are not available. The workflow installs it on GitHub's servers. If you run the script locally and it can't find Open Sans, put `OpenSans-Regular.ttf` and `OpenSans-Bold.ttf` in a `fonts/` folder and the script will use those.

## Not official
This is a fan-made helper. It is not made or endorsed by Extra Life or Children's Miracle Network.
If you appreciate this helper, please [contribute to the developer's CMN Extra-Life Campaign](https://dd.extra-life.org/participant/DDOwiki) and be a CHAMPION!
