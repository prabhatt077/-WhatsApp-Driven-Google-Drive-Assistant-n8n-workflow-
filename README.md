
# WhatsApp → Google Drive Assistant (n8n workflow)

This project is an **n8n workflow** that connects **WhatsApp** (via Twilio Sandbox) to **Google Drive** for performing actions like **listing files, deleting, moving, and summarising** documents.

---

## 1️⃣ Twilio Sandbox Setup

1. **Log in** to your [Twilio Console](https://www.twilio.com/console).
2. Navigate to **Messaging → Try it Out → Send a WhatsApp message**.
3. Click **“Sandbox”** and follow the instructions:

   * Scan the QR code or send the provided join code to `+14155238886` on WhatsApp.
   * Verify your number is connected to the sandbox.
4. Copy the **Sandbox webhook URL** from Twilio settings.
5. **In n8n**, open your workflow and copy the **Webhook node URL**.
6. Paste the n8n webhook URL into Twilio’s **"WHEN A MESSAGE COMES IN"** field.

📌 **Tip:** While testing, remember the sandbox can only send/receive from numbers that joined via the code.

---

## 2️⃣ Google Credentials Setup

1. Go to [Google Cloud Console](https://console.cloud.google.com/).
2. Create a **New Project** → enable the **Google Drive API**.
3. Navigate to **APIs & Services → Credentials → Create OAuth Client ID**.
4. Choose:

   * **Application Type**: Web application
   * Add **Authorized Redirect URI**:

     ```
     (http://localhost:5678/rest/oauth2-credential/callback)
     ```
5. Download the OAuth client JSON and note:

   * **Client ID**
   * **Client Secret**
6. In **n8n → Credentials → Google Drive OAuth2 API**, enter:

   * Client ID
   * Client Secret
   * Redirect URI (same as above)
7. Connect & Authorize — n8n will store the refresh token automatically.

---

## 3️⃣ Command Syntax

The bot accepts **slash-style commands** via WhatsApp. Commands are **case-insensitive** and tolerate extra spaces.

| Command         | Syntax                                          | Example                              | Action                               |
| --------------- | ----------------------------------------------- | ------------------------------------ | ------------------------------------ |
| **List files**  | `LIST /<FolderPath>`                            | `LIST /ProjectX`                     | Lists files in given folder          |
| **Delete file** | `DELETE /<FolderPath>/<FileName>`               | `DELETE /ProjectX/report.pdf`        | Deletes file (requires confirmation) |
| **Move file**   | `MOVE /<FolderPath>/<FileName> /<TargetFolder>` | `MOVE /ProjectX/report.pdf /Archive` | Moves file to another folder         |
| **Summary**     | `SUMMARY /<FolderPath>`                         | `SUMMARY /LegalDocs`                 | Summarises each file’s contents      |

**Delete Confirmation Flow:**

```
User: DELETE /ProjectX/old.docx
Bot: Are you sure? Reply with:
CONFIRM DELETE /ProjectX/old.docx
```

---

