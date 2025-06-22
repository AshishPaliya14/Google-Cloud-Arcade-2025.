# Google-Cloud-Arcade-2025

# Google Apps Script: Access Google Sheets, Maps & Gmail in 4 Lines of Code || Lab Solution || GSP235 || Google Cloud Arcade 2025 🎮

## 💡 Solution here

📍 Step 1 : Click into the first cell in the upper left-hand corner (A1). It should be in column A and row 1:

```
28 Snowbird Lane, Wasilla,ak, 99683  United States
```

📍 Step 2: From the top menu bar, select **Extensions** > **Apps Script**

📍 Step 3 : **Update Code.gs with Custom Script Logic**
``` 
function sendMap() {
    var sheet = SpreadsheetApp.getActiveSheet();
    var address = sheet.getRange("A1").getValue();
    var map = Maps.newStaticMap().addMarker(address);
    GmailApp.sendEmail("<YOUR_EMAIL>", "Map", 'See below.', {attachments:[map]});
}
```
---

### 🎉 Woohoo! You Did It! 🎉

Your hard work and determination paid off! 💻  
You've successfully completed the lab. Way to go! 🚀  

### 💬 Stay Connected with Our Community!


## Subscribe :  [Learn With Ashish](https://www.youtube.com/channel/UChSkWopRk1ErP2i0k4aa0KQ)
