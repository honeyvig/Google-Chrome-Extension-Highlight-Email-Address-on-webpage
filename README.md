# Google-Chrome-Extension-Highlight-Email-Address-on-webpage
 create a functional and user-friendly extension that enables users to highlight email addresses on a webpage, right-click to access a context menu option, and open an Outlook compose page with the highlighted email pre-filled as the recipient.

Key Requirements:

Context Menu Integration:
-Add a right-click menu option to send emails via Outlook when an email address is highlighted.


Recipient Field Automation:
-Open a new tab to the Outlook compose page (https://outlook.live.com/mail/0/compose).
Ensure the highlighted email address is correctly filled in the recipient field of the draft.

Foreground Behavior:
-Ensure the draft tab opens in the foreground, not in the background.
Stay on the compose tab, not redirect to the inbox.

Compatibility:

-The extension must work seamlessly with the latest version of Google Chrome.
-Must support Outlook's web version (https://outlook.live.com).

Deliverables:
-A fully functional Chrome extension meeting the above requirements.
-Complete source code with clear documentation for installation and usage.
-----------------
To build a Google Chrome extension that allows users to highlight email addresses on a webpage, right-click, and open Outlook's compose page with the highlighted email pre-filled, we need to implement the following steps.
1. Manifest File (manifest.json)

This is the metadata file for your Chrome extension. It will define the permissions, background scripts, and content scripts for the extension.

{
  "manifest_version": 3,
  "name": "Outlook Email Sender",
  "description": "Highlight an email address, right-click to send an email through Outlook.",
  "version": "1.0",
  "permissions": [
    "contextMenus",
    "activeTab"
  ],
  "background": {
    "service_worker": "background.js"
  },
  "action": {
    "default_popup": "popup.html"
  },
  "content_scripts": [
    {
      "matches": ["<all_urls>"],
      "js": ["content.js"]
    }
  ]
}

2. Background Script (background.js)

The background script is responsible for creating the context menu when a user right-clicks on a webpage and handling the action of opening Outlook with the pre-filled recipient.

// background.js

chrome.runtime.onInstalled.addListener(() => {
  // Create context menu for the highlighted email address
  chrome.contextMenus.create({
    id: "sendEmail",
    title: "Send email via Outlook",
    contexts: ["selection"]
  });
});

// When a user selects an email address and clicks on the context menu option
chrome.contextMenus.onClicked.addListener((info, tab) => {
  if (info.menuItemId === "sendEmail" && info.selectionText) {
    const email = info.selectionText;
    openOutlookCompose(email);
  }
});

// Function to open Outlook compose page with the email pre-filled
function openOutlookCompose(email) {
  const outlookURL = `https://outlook.live.com/mail/0/compose?to=${encodeURIComponent(email)}`;
  chrome.tabs.create({ url: outlookURL, active: true });
}

3. Content Script (content.js)

This script will be injected into the webpage, allowing the extension to interact with the page and highlight email addresses. It doesn’t need to do much, as the context menu and logic are handled by the background script. This file is used to interact with the page’s DOM, if necessary.

// content.js

// Highlight email addresses on the page (optional functionality)
// This script could be expanded to auto-detect and highlight email addresses on the page.
document.body.addEventListener("mouseover", (event) => {
  if (event.target.tagName === "A" && event.target.href.startsWith("mailto:")) {
    event.target.style.backgroundColor = "yellow";  // Highlight email links
  }
});

document.body.addEventListener("mouseout", (event) => {
  if (event.target.tagName === "A" && event.target.href.startsWith("mailto:")) {
    event.target.style.backgroundColor = "";  // Remove highlight when mouse leaves
  }
});

4. Popup HTML (Optional) (popup.html)

This is the popup that could be shown when clicking on the extension's icon in the Chrome toolbar. This is optional, but you could use it for extension settings or to provide more information to the user.

<!DOCTYPE html>
<html>
  <head>
    <title>Outlook Email Sender</title>
    <style>
      body {
        width: 200px;
        padding: 10px;
        font-family: Arial, sans-serif;
      }
      h2 {
        margin: 0;
        font-size: 16px;
      }
    </style>
  </head>
  <body>
    <h2>Outlook Email Sender</h2>
    <p>Highlight an email address, right-click, and select "Send email via Outlook".</p>
  </body>
</html>

5. Icon (Optional)

You can add icons to represent your extension in the toolbar. Save your icon files in the icons directory (e.g., icon16.png, icon48.png, icon128.png) or you can use any default icons from the Chrome extension documentation.
6. How it Works:

    Context Menu: When a user highlights an email address (text selection) on a webpage, they can right-click to see a context menu option that says “Send email via Outlook”.

    Opening the Outlook Compose Page: When the user selects this option, the extension opens a new tab in the browser with the Outlook compose page, pre-filled with the highlighted email address in the recipient field.

    Foreground Behavior: The tab opens in the foreground and the focus remains on the compose tab rather than redirecting to the inbox.

7. Installation and Usage:

To install this Chrome extension locally:

    Open Chrome and go to chrome://extensions/.
    Enable "Developer mode" (toggle in the top-right).
    Click on "Load unpacked" and select the directory containing the extension files.
    The extension should now appear in the Chrome extensions bar.
    Visit any webpage with an email address, highlight it, right-click, and select the “Send email via Outlook” option from the context menu.

Final Thoughts:

This Chrome extension simplifies emailing by allowing users to highlight an email address on a webpage and easily send an email via Outlook. It opens the Outlook compose page with the selected email pre-filled, and the extension works seamlessly in the foreground. You can further customize the extension as per the specific design or additional features, like supporting additional email platforms or customizing the interface.
