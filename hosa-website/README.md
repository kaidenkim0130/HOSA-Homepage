# HOSA Website — Setup Guide

A simple website for your HOSA chapter with a homepage slideshow, an officers page, a resources link, and a live points lookup powered by Google Sheets.

## What's in this folder

| File | What it is |
|------|------------|
| `index.html` | Homepage with the 7-photo slideshow |
| `officers.html` | "Meet the Officers" page |
| `points.html` | The points search page (connects to your Google Sheet) |
| `styles.css` | All the styling — shared by every page |
| `images/` | Folder where you put your slideshow photos |

---

## 1. Add your 7 slideshow photos

1. Create a folder called **`images`** in the same place as `index.html`.
2. Drop your 7 photos in there and name them:
   - `slide1.jpg`
   - `slide2.jpg`
   - `slide3.jpg`
   - `slide4.jpg`
   - `slide5.jpg`
   - `slide6.jpg`
   - `slide7.jpg`
3. That's it — the homepage will pick them up automatically. (If a photo isn't there yet, you'll see a dark placeholder instead, which is fine.)

> If your photos are `.png` files instead of `.jpg`, just open `index.html` and change the `data-src` paths from `slide1.jpg` to `slide1.png`, etc.

---

## 2. Connect your Google Sheet to the Points page

This is the most important step. Here's how to do it:

### Step 1 — Set up your sheet

Open your Google Sheet (or make a new one). The first row needs to be column headers in this order:

| A | B | C |
|---|---|---|
| Name | Sem 1 Points | Sem 2 Points |

Then add a row for each member, like:

| Name | Sem 1 Points | Sem 2 Points |
|------|--------------|--------------|
| Sohyeon K. | 55 | 45 |
| Jane Doe | 30 | 20 |

You **don't** need a "Total" column — the website adds Sem 1 + Sem 2 for you.

### Step 2 — Publish it

In Google Sheets, click:

**File → Share → Publish to web**

In the dialog that appears:
- Under "Link", pick your sheet/tab
- Change the dropdown from "Web page" to **"Comma-separated values (.csv)"**
- Click **Publish**, then **OK**
- **Copy** the URL it gives you

### Step 3 — Paste the URL into points.html

Open `points.html` in a text editor and find this line (it's near the top of the `<script>` section):

```js
const SHEET_CSV_URL = "PASTE_YOUR_PUBLISHED_CSV_URL_HERE";
```

Replace `PASTE_YOUR_PUBLISHED_CSV_URL_HERE` with the URL you copied. Keep the quotes around it.

**Done!** You can now edit the Google Sheet anytime and the website will show the latest data. (Google may take a minute or two to refresh after you save changes in the sheet.)

---

## 3. Add your Google Doc link (the "Resources" tab)

Once your Google Doc is ready:

1. Open the doc and click **Share** (top right). Make sure it's set to "Anyone with the link can view".
2. Copy the doc's URL from your browser's address bar.
3. Open each of `index.html`, `officers.html`, and `points.html` in a text editor.
4. Find this line in the nav bar:
   ```html
   <li><a href="#" target="_blank" rel="noopener">Resources</a></li>
   ```
5. Replace the `#` with your doc URL. For example:
   ```html
   <li><a href="https://docs.google.com/document/d/abc123/edit" target="_blank" rel="noopener">Resources</a></li>
   ```
6. Save all three files. Done!

> Tip: If you'd rather call the tab something other than "Resources" (like "Member Handbook" or "Docs"), just change that word too.

---

## 4. Edit the officers

Open `officers.html` and scroll down to the big comment that says **👇 EDIT YOUR OFFICERS HERE 👇**. Below it you'll see a list that looks like this:

```js
const OFFICERS = [
  {
    name: "Jane Doe",
    role: "President",
    photo: "",
    bio: "Senior · A short sentence about Jane and why she loves HOSA."
  },
  {
    name: "John Smith",
    role: "Vice President",
    photo: "",
    bio: "Junior · A short sentence about John..."
  },
  // ... more officers ...
];
```

Each officer is one block between `{` and `}`. To update someone, just change the text between the quotes.

**Fields explained:**
- `name` — The officer's name shown on the card
- `role` — Their position (President, VP, etc.)
- `photo` — Path to their picture, like `"images/jane.jpg"`. Leave as `""` to show their initials instead.
- `bio` — A short sentence about them

**To add a photo for someone:**
1. Put their photo in the `images` folder (e.g. `images/jane.jpg`).
2. In their block, change `photo: ""` to `photo: "images/jane.jpg"`.

**To add a new officer:** copy a whole block (from `{` to `},` including the comma) and paste it inside the `[ ]`. Then edit the values.

**To remove an officer:** delete their whole block (from `{` to `},`).

> Important: keep the commas, the quotes, and the curly braces exactly as they are — those are how the page knows where one officer ends and the next begins.

---

## 5. Deploy

Since you're already using Vercel, just drop this whole folder into your project (or drag-and-drop it onto vercel.com) and it'll work. No build step needed.

---

## Troubleshooting

**The points page says "isn't connected to a Google Sheet yet."**
You haven't pasted your published CSV URL into `points.html`. See section 2.

**The points page says "No results found"**
Check that the name you typed actually exists in the sheet. Search matches anywhere in the name, so "Sohyeon" will find "Sohyeon K."

**I edited my sheet but the website still shows the old numbers.**
Google's published CSV refreshes every few minutes — not instantly. Wait 1–5 minutes and refresh the page. If it's still stuck, go back to **File → Share → Publish to web** and make sure it's still published.

**The slideshow only shows dark placeholders.**
The images aren't in the `images/` folder, or they have different filenames. See section 1.
