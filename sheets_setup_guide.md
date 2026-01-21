# Google Sheets Voting App - Setup Guide

## Overview
This guide will help you set up Google Sheets as the backend for your plot voting app. Total time: ~10-15 minutes.

---

## Step 1: Create Your Google Sheet

1. Go to [Google Sheets](https://sheets.google.com)
2. Create a new blank spreadsheet
3. Name it something like "Plot Voting Data"
4. In **Row 1**, add these exact headers (case-sensitive):

   ```
   Plot_ID | Voter | Vote | Filename | Timestamp | Sheet_URL
   ```

5. **Important**: Name the sheet tab "Votes" (bottom left, double-click to rename)

6. Copy the **Spreadsheet ID** from the URL:
   ```
   https://docs.google.com/spreadsheets/d/SPREADSHEET_ID_HERE/edit
                                            ^^^^^^^^^^^^^^^^
   ```
   Save this ID - you'll need it later!

---

## Step 2: Enable Google Sheets API

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project (or select existing)
   - Click the project dropdown at the top
   - Click "New Project"
   - Name it "Plot Voting App"
   - Click "Create"

3. Enable the Google Sheets API:
   - In the left menu, go to **APIs & Services** → **Library**
   - Search for "Google Sheets API"
   - Click on it, then click **"Enable"**

---

## Step 3: Create an API Key

1. In Google Cloud Console, go to **APIs & Services** → **Credentials**
2. Click **"+ Create Credentials"** → **"API Key"**
3. Your API key will be generated - **copy it immediately**
4. Click **"Edit API key"** (or the pencil icon)
5. Under "API restrictions":
   - Select **"Restrict key"**
   - Check **"Google Sheets API"**
   - Click **"Save"**

6. **Security Note**: This API key will be visible in your HTML file. To keep it secure:
   - Only share the GitHub repo with trusted collaborators
   - OR use Google Apps Script (more advanced) for better security

---

## Step 4: Make Sheet Publicly Readable

1. Open your Google Sheet
2. Click the **"Share"** button (top right)
3. Click **"Change to anyone with the link"**
4. Set permissions to **"Viewer"**
5. Click **"Done"**

**Important**: The sheet must be publicly readable for the API to work!

---

## Step 5: Configure the HTML File

1. Open the `index.html` file in a text editor
2. Find the `SHEETS_CONFIG` section near the top (around line 13):

   ```javascript
   const SHEETS_CONFIG = {
       apiKey: 'YOUR_API_KEY_HERE',
       spreadsheetId: 'YOUR_SPREADSHEET_ID_HERE',
       range: 'Votes!A:F'
   };
   ```

3. Replace:
   - `YOUR_API_KEY_HERE` with your API key from Step 3
   - `YOUR_SPREADSHEET_ID_HERE` with your Spreadsheet ID from Step 1

4. Save the file

---

## Step 6: Deploy to GitHub Pages

1. Create a GitHub repository with this structure:
   ```
   your-repo/
   ├── plots/
   │   ├── plot_0001.png
   │   ├── plot_0002.png
   │   └── ... (all 1000 images)
   └── index.html  (your configured file)
   ```

2. Push to GitHub

3. Enable GitHub Pages:
   - Go to repo **Settings** → **Pages**
   - Under "Source", select **"main"** branch
   - Click **Save**

4. Your app will be live at:
   ```
   https://username.github.io/repo-name/
   ```

---

## Step 7: Configure Image Path

1. Open your deployed app
2. Click **"Settings"** button
3. Enter the image base path:
   ```
   ./plots
   ```
   (or the full URL if images are hosted elsewhere)
4. Click **"Save Settings"**

---

## How It Works

### Data Flow:
1. User votes → Appended as new row to Google Sheet
2. Clicking "Sync" or "Stats" → Reads all data from Sheet
3. Stats page → Shows aggregated vote counts
4. "Open Sheet" button → View/export raw data

### Google Sheet Structure:
```
Plot_ID | Voter  | Vote | Filename        | Timestamp           | Sheet_URL
--------|--------|------|-----------------|---------------------|----------
1       | Alice  | Yes  | plot_0001.png   | 2024-01-15T10:30:00 | [link]
1       | Bob    | No   | plot_0001.png   | 2024-01-15T10:31:00 | [link]
2       | Alice  | Yes  | plot_0002.png   | 2024-01-15T10:32:00 | [link]
```

### Features:
- ✅ All votes stored in Google Sheets
- ✅ Direct spreadsheet access for analysis
- ✅ Export to CSV/Excel from Sheets
- ✅ Filter, sort, pivot in Sheets
- ✅ Multiple people can vote simultaneously
- ✅ Manual sync with "Refresh" button

---

## Troubleshooting

### "Failed to load data" error
- Check that your API key is correct
- Verify the Sheet ID matches your spreadsheet
- Ensure the sheet is set to "Anyone with the link can view"
- Check that the sheet tab is named "Votes"

### "Failed to save vote" error
- Make sure Google Sheets API is enabled
- Check your API key restrictions include Google Sheets API
- Verify your internet connection

### Images not loading
- Check the image base path in Settings
- Ensure filenames match: `plot_0001.png`, `plot_0002.png`, etc.
- Verify images are in the `/plots` folder

### Votes not syncing
- Click the "Sync" button to refresh data
- Google Sheets API has rate limits (100 requests per 100 seconds per user)
- If many people voting at once, there may be a slight delay

---

## Tips for Best Experience

1. **Periodic Syncing**: Click "Sync" button occasionally to see others' votes
2. **Direct Sheet Access**: Use "Open Sheet" button to view/export all data
3. **Data Analysis**: Use Google Sheets pivot tables, charts, and formulas
4. **Backups**: Make a copy of your sheet periodically
5. **Sharing**: Share the deployed URL with your team

---

## Advanced: Better Security

If you want to keep your API key private:

1. Use Google Apps Script as a proxy
2. Deploy script as web app
3. Update HTML to call your script endpoint
4. Script handles API authentication server-side

This requires more setup but keeps credentials secure. Let me know if you need help with this!

---

## Need Help?

Common issues and questions:
- **Q: Can I change the filename pattern?**
  - A: Yes! Edit line ~134 in the HTML where filenames are generated
  
- **Q: Can I have more/fewer than 1000 plots?**
  - A: Yes! Change the loop on line ~132: `for (let i = 1; i <= YOUR_NUMBER; i++)`
  
- **Q: Can I add more columns to the sheet?**
  - A: Yes, but you'll need to modify the `saveVote` function to include additional data

---

Good luck with your voting! 🎉