---
title: "Alerting and reporting with Slack"
description: "How to send automatic Slack notifications from Google Sheets"
lead: "How to send automatic Slack notifications from Google Sheets"
date: 2021-01-01
lastmod: 2021-01-01
draft: false
images: []
menu:
  docs:
    parent: "Section 5: Monitoring and alerting"
weight: 700
toc: true
---

{{< alert icon="💻" context="info" text="<b>TL;DR</b> Sometimes we need to pull information from our Google Spreadsheets, Bigquery or other sources and send it to a Slack message. In this tutorial you will learn how to set up automatic alerts and reports to send to Slack. We'll use some simple Google Apps Script functions, along with the Slack API. Let's get started! " />}}

## Creating a custom app in Slack

* Go to [https://api.slack.com](https://api.slack.com). Everything you need to know about the Slack API is here.
* In the main nav, click on `Your Apps`. Followed by `Create new app`.
* In the modal loaded, you can provide de App name and select your company's development workspace. Finally, hit `Create App`.
* Now, in the left hand menu select `Incoming Webhooks`.
* Toggle `Activate Incoming Webhooks` to on, and then click the `Add New Webhook to Workspace` button.
* Finally, select the Slack channel that you want the Slack alert to appear in and click `Allow`

## Fetch data from your spreadsheet using Google Apps Script

* *Rowan Barnes* has set up a simple Google spreadsheet with dummy data – feel free to make a copy of [this spreadsheet](https://docs.google.com/spreadsheets/d/1h8Q4KWsqxFhYQnT697Rlru1uC8bNN9KNNI5CIEu-SRs/edit#gid=0) (make sure you are logged into a Google account and then go to File > Make a copy), or alternatively use your own Google spreadsheet (either is fine).
* Once your spreadsheet is ready, you need to access Google Apps Script. To do that, click Tools > Script editor
* To get started, give your script a meaningful name, and then replace the empty function at the top with the following:

```
function buildReport() {
  const ss = SpreadsheetApp.getActive();

  // getting the data from cells
  let data = ss.getSheetByName('Data').getRange("A1:B6").getValues();

  // the function sends that data to another function called ‘buildAlert’ 
  let payload = buildAlert(data);

  // then sends the returned data from the buildAlert function to another function called ‘sendAlert’
  sendAlert(payload);
}

```
{{< alert icon="⚠️" context="warning" text="If you copied the template spreadsheet above, all of the required code will already be in place (well, all apart from your webhook URL, which we’ll get to in step 4)." />}}


## Transform the data into a format understandable by Slack

The data you just fetched in the previous step is in JavaScript array format. You can’t send it to Slack in this format – Slack won’t understand it. Instead, it needs to be converted into specially-structured JSON that Slack will understand. Slack calls this structure `Blocks`. **Slack’s documentation is great, so I won’t repeat it here**. Simply take a quick peek at the link above, and then come back here.

```
function buildAlert(data) {
  let totalRevenue = data[0][1];
  let revenueBreakdown = data.slice(1).map(function(row) {
    return row[0] + ": $" + row[1];
  }).join("\n");

  let payload = {
    "blocks": [

        // The alert title
      {
        "type": "section",
        "text": {
          "type": "mrkdwn",
          "text": ":bell: *Fundraising Revenue Report* :bell:"
        }
      },
      // Horizontal divider below the title
      {
        "type": "divider"
      },

    // Print a constant from cell A1
      {
        "type": "section",
        "text": {
          "type": "mrkdwn",
          "text": "Our total fundraising revenue for the past month was $" + totalRevenue
        }
      },
      {
        "type": "section",
        "text": {
          "type": "mrkdwn",
          "text": "A breakdown of revenue by source is as follows:"
        }
      },
      {
        "type": "section",
        "text": {
          "type": "mrkdwn",
          "text": revenueBreakdown
        }
      }
    ]
  };
  return payload;
}
```
{{< alert icon="🎉" context="success" text="Tip: Slack’s [Block Kit Builder](https://app.slack.com/block-kit-builder/) is an awesome place to experiment with the JSON format. Play around with things in there before you set up your ‘payload’ variable in Apps Script." />}}

The first line in our function simply gets your `total revenue` figure from your data, which is in the first row and second column of our data array (remembering that JavaScript arrays are zero-based i.e. the ‘0’ = row 1 and the ‘1’ = column 2).

The subsequent code block takes the remaining data in the array (representing range A2:B6 in the spreadsheet) and turns it from an array into a text string, formatted the way we want:

For those of you using your own data, you might need to play around with this section of code to get things looking right.

## :mailbox_with_mail: Send the alert to Slack

Our final function is your `sendAlert` function, which – you guessed it – sends the formatted alert to Slack. Here it is:

```
function sendAlert(payload) {
  const webhook = ""; //Paste your webhook URL here
  var options = {
    "method": "post", 
    "contentType": "application/json", 
    "muteHttpExceptions": true, 
    "payload": JSON.stringify(payload) 
  };
  
  try {
    UrlFetchApp.fetch(webhook, options);
  } catch(e) {
    Logger.log(e);
  }
}

```
The only part you need to do is paste in your webhook URL that you created in step 1. You can get that from your app’s ‘incoming webhooks’ page that we talked about in step 1

The final step is to give it a test. In Apps Script, select the 'buildReport' function from the toolbar, and click the Run icon. Note that the first time you do this, Apps Script will ask you for permission to run the script.

Once you’ve accepted all the permissions and the script has run, head on over to Slack, and voila!, the Slack alert should be sitting there in the channel you chose in step 1.

## Set up scheduled alerts

Setting up scheduled alerts is incredibly useful. Imagine you want to send an alert once per week. You can set up a ‘trigger’ within Apps Script to make that happen.

{{< alert icon="☠️" context="danger" text="Finally, remember to never share a private key from any of your services in your work repository, on the google sheet or in Slack. It's obvious, but don't ;) " />}}

---

_**Note**: To create this tutorial I have based on the following [document](https://www.august.com.au/blog/how-to-send-slack-alerts-from-google-sheets-apps-script/) __written by Rowan Barnes__, to which I have added some comments or explanations that I find useful to go further on the subject._


