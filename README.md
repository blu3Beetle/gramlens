# GramLens

**Privacy-first Instagram analytics — runs entirely in your browser. No server. No account. No tracking.**

GramLens lets you analyse your Instagram connections and engagement using your own data export. Everything happens locally on your device — your data never leaves your machine.

> Instagram shut down third-party analytics apps that used live API access. GramLens works differently: you export your own data directly from Instagram, and GramLens reads it locally in your browser.

---

## Table of contents

- [Privacy guarantee](#privacy-guarantee)
- [Requirements](#requirements)
- [How to use GramLens](#how-to-use-gramlens)
- [How to export your Instagram data](#how-to-export-your-instagram-data)
- [Setting up your folders](#setting-up-your-folders)
- [Connections tabs explained](#connections-tabs-explained)
- [Engagement tabs explained](#engagement-tabs-explained)
- [How the baseline works](#how-the-baseline-works)
- [Influencer vs Deactivated](#influencer-vs-deactivated)
- [Known limitations](#known-limitations)

---

## Privacy guarantee

GramLens is a single HTML file. When you open it in your browser:

- **No data is uploaded anywhere.** Your Instagram export stays on your device.
- **No analytics or tracking.** There are no scripts calling any external server.
- **No account required.** Nothing is stored outside the two local folders you choose.
- **Open source.** You can read every line of code in `index.html`.

The only network activity is the initial page load from GitHub Pages (serving the HTML file itself). After that, everything is local.

---

## Requirements

- **Chrome or Edge** (desktop) — required for the File System Access API, which is what lets the app read your export folder and write your baseline file without uploading anything.
- Firefox and Safari do not currently support this API and will not work.
- No installation, no Node.js, no terminal. Just open the page and go.

---

## How to use GramLens

1. Open **[GramLens](https://blu3beetle.github.io/gramlens)** in Chrome or Edge
2. Export your Instagram data (see below)
3. Click **Select export folder** and point it to your extracted export folder
4. Click **Select workspace folder** and point it to your GramLens workspace folder
5. Click **Run analysis**
6. Browse your connections and engagement tabs
7. When done, click **Save snapshot as new baseline** — this lets GramLens show you diffs next time

---

## How to export your Instagram data

Instagram's data export can take anywhere from a few minutes to a few hours depending on account size.

### Step by step

1. Open Instagram and go to **Settings**
2. Tap **Accounts Centre**
3. Tap **Your information and permissions**
4. Tap **Download your information**
5. Tap **Download or transfer information**
6. Select your **Instagram account**
7. Select **Some of your information**
8. Select only the following (everything else can be left unchecked):

### What to select

Under **Your Instagram activity:**
- ✅ Comments
- ✅ Likes
- ✅ Saved
- ✅ Story interactions

Under **Connections:**
- ✅ Followers and following

Leave everything else unchecked. This keeps the export small and fast — GramLens doesn't use any of the other data.

### Export settings

| Setting | Value |
|---|---|
| Date range | All time |
| Format | **JSON** ← important, must be JSON not HTML |
| Media quality | Low (GramLens doesn't use media files) |
| Notify | Your email |

9. Tap **Create export** → **Export to device**
10. You'll receive two emails from Meta Privacy Team:
    - First email: confirms your request was received
    - Second email: confirms your export is ready to download (can take minutes to hours)
11. Download the ZIP from the link in the second email
12. **Extract the ZIP** to a folder on your computer — this is your export folder

---

## Setting up your folders

GramLens needs access to two folders on your machine.

### Export folder

Point this to the folder you extracted from Instagram's ZIP. GramLens navigates it automatically to find the files it needs — you don't have to select individual files.

The app looks for files at these paths inside your export folder:
- `connections/followers_and_following/following.json`
- `connections/followers_and_following/followers_1.json`
- `connections/followers_and_following/recently_unfollowed_profiles.json`
- `connections/followers_and_following/close_friends.json`
- `connections/followers_and_following/restricted_profiles.json`
- `connections/followers_and_following/blocked_profiles.json`
- `connections/followers_and_following/hide_story_from.json`
- `connections/followers_and_following/pending_follow_requests.json`
- `your_instagram_activity/likes/liked_posts.json`
- `your_instagram_activity/likes/liked_comments.json`
- `your_instagram_activity/comments/post_comments_1.json`
- `your_instagram_activity/story_interactions/stories_viewed.json`
- `your_instagram_activity/story_interactions/story_likes.json`
- `your_instagram_activity/saved/saved_posts.json`

### Workspace folder

This is a folder GramLens uses to store your analysis history. It writes:
- `baseline.json` — your saved snapshot, used to compare against next time
- `archive/` — timestamped snapshots of previous analyses

**First time:** Create a new empty folder anywhere on your machine (e.g. `Documents/GramLens`) and select it.

**Returning:** Always select the same folder so your history carries over.

> The browser will ask you to grant folder access each time you open GramLens — this is a browser security requirement and is expected.

---

## Connections tabs explained

| Tab | What it shows |
|---|---|
| **Unfollowers** | Accounts you follow that don't follow you back. Excludes influencers, deactivated accounts, and accounts awaiting your follow request approval. |
| **You don't follow** | Accounts that follow you but you don't follow back. |
| **New friends** | Accounts that became mutual follows since your last saved baseline. |
| **Lost friends** | Accounts that were mutual follows before but neither of you follow each other anymore. |
| **Friends** | Current mutual follows that existed in your previous baseline too (not new). |
| **Close friends** | Your Instagram close friends list. |
| **Influencers** | Accounts you've manually marked as influencers — they're excluded from Unfollowers since you don't expect them to follow back. Star icon to mark/unmark. |
| **Deactivated** | Accounts you've marked as deactivated — excluded from Unfollowers since their page doesn't load. ⊘ icon to mark/unmark. If they reactivate and follow you back, they'll automatically move to New friends. |
| **Restricted** | Accounts you've restricted on Instagram. |
| **Blocked** | Accounts you've blocked on Instagram. |
| **Awaiting approval** | Accounts you sent a follow request to that haven't accepted yet (their account is private). These are excluded from Unfollowers since they haven't had a chance to follow back. |
| **Hidden story from** | Accounts you've hidden your stories from. |
| **You unfollowed** | Accounts you recently unfollowed, according to Instagram's log. |

---

## Engagement tabs explained

These tabs show which accounts you engage with most, ranked by frequency.

| Tab | What it shows |
|---|---|
| **Most liked (posts)** | Accounts whose posts you like the most. |
| **Most liked (comments)** | Accounts whose comments you've liked. |
| **Most commented** | Accounts whose posts you comment on most. |
| **Most watched stories** | Accounts whose stories you view most frequently. |
| **Story likes** | Accounts whose stories you've liked. |
| **Most saved from** | Accounts whose posts you save the most. |

Cross-referencing these with your Unfollowers or You don't follow tabs can surface accounts you interact with heavily but don't have a mutual connection with.

---

## How the baseline works

A baseline is a saved snapshot of your following and followers at a point in time.

- **First run:** No baseline exists yet. All mutual follows are shown as Friends (not New friends), since there's nothing to compare against.
- **After saving:** Click **Save snapshot as new baseline** when you're done reviewing. GramLens writes `baseline.json` to your workspace folder.
- **Next run:** GramLens compares the new export against the saved baseline and shows you New friends, Lost friends, and Unfollowers as diffs since the last save.

The more regularly you run GramLens and save a baseline, the more useful the diffs become.

---

## Influencer vs Deactivated

Both categories remove accounts from your Unfollowers count, but for different reasons:

**Influencer** — you follow this account knowing they'll never follow back (celebrities, brands, creators). Star (☆/★) to mark from the Unfollowers tab.

**Deactivated** — this account's page doesn't load, probably temporarily deactivated. ⊘ to mark from the Unfollowers tab. If they come back and follow you, GramLens automatically detects this and moves them to New friends on the next run.

---

## Known limitations

- **Chrome and Edge only** — the File System Access API is not supported in Firefox or Safari.
- **No profile pictures** — Instagram's export doesn't include avatar images and GramLens can't fetch them without authentication.
- **Username changes** — if someone changes their Instagram handle, GramLens loses track of them since the export only contains usernames, not internal account IDs.
- **followers_1.json only** — if your account has more than ~500 followers, Instagram may split the followers file into `followers_1.json`, `followers_2.json`, etc. GramLens currently only reads `followers_1.json`. Support for multiple files coming soon.
- **Export delay** — Instagram exports are a point-in-time snapshot and can take hours to generate. The data may be slightly behind real-time.
