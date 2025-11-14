**📅 Meeting Scheduler Workflow (n8n)

An automated Meeting Scheduler Agent built using n8n.
This workflow reads meeting details from Google Sheets, processes them with custom logic, and automatically sends email notifications using Gmail — all without manual effort.

📌 Overview

This workflow helps you automate:

✔ Sending meeting reminders
✔ Notifying participants of upcoming events
✔ Fetching scheduled meetings from Google Sheets
✔ Running on a fixed schedule (e.g., every day/morning/hour)

It is useful for office teams, personal reminders, educational institutions, and automated communication systems.

🏗 Workflow Architecture
┌────────────────┐       ┌──────────────────────┐      ┌───────────────┐      ┌────────────────┐
│ Schedule       │ ----> │ Google Sheet Reader  │ ---->│  Code Node     │ ---->│ Gmail Sender   │
│ Trigger        │       │ (Get rows)           │      │ (processing)   │      │ (Send Email)   │
└────────────────┘       └──────────────────────┘      └───────────────┘      └────────────────┘

📂 Features

Automated Scheduler – runs at your preferred interval

Google Sheets Integration – fetches data like meeting time, participants, message, etc.

Code Logic – filter meetings, build email content

Gmail Sender – sends personalized emails automatically

Fully Customizable – adjust timing, message format, Google Sheet structure, etc.

📄 Example Google Sheet Format

Your Google Sheet should follow this structure:

Meeting Title	Date	Time	Participants	Message
Team Sync	2025-01-12	10:00 AM	team@example.com
	Monthly sync-up meeting
Review Call	2025-01-13	04:00 PM	hr@example.com
	Performance review reminder
⚙️ Node-by-Node Explanation
1. Schedule Trigger

Runs the workflow automatically
Example: every day at 8 AM

2. Google Sheet → Get Row(s)

Reads upcoming meeting details from your sheet
Requires:

Google API credentials

Sheet ID + Range

3. Code Node

Processes fetched data using JavaScript logic.

Sample Code:

// Get rows from previous node
const rows = $input.all();

// Prepare filtered meetings (e.g., today’s meetings)
let filtered = [];

rows.forEach(item => {
    const data = item.json;

    const meetingDate = new Date(data.Date);
    const today = new Date();

    // Example condition: send reminders only for today
    if (
        meetingDate.getFullYear() === today.getFullYear() &&
        meetingDate.getMonth() === today.getMonth() &&
        meetingDate.getDate() === today.getDate()
    ) {
        filtered.push({
            subject: `Reminder: ${data['Meeting Title']}`,
            email: data.Participants,
            message:
`Hi,

This is a reminder for your meeting:

Title: ${data['Meeting Title']}
Date: ${data.Date}
Time: ${data.Time}

${data.Message}

Regards,
Meeting Scheduler Bot`
        });
    }
});

return filtered.map(item => ({ json: item }));

4. Gmail → Send Message

Uses Gmail to send an email based on the output of the Code node.

Example fields:

To: {{$json["email"]}}

Subject: {{$json["subject"]}}

Message: {{$json["message"]}}

🧰 Requirements
Component	Requirement
n8n	Cloud, Desktop, or Self-hosted
Google Sheets API	Credentials + shared sheet
Gmail API	OAuth2 or App Password
Spreadsheet	Correct column format
🚀 Installation

Clone the repository:

git clone https://github.com/<your-user>/<your-repo>.git


Open n8n → Import workflow.json

Add:

Google Sheets credentials

Gmail credentials

Insert your Google Sheet ID into the Google Sheet node

Update your schedule timing

Activate workflow ✓

📨 Example Email Output
Subject: Reminder: Team Sync

Hi,

This is a reminder for your meeting:

Title: Team Sync
Date: 2025-01-12
Time: 10:00 AM

Monthly sync-up meeting

Regards,
Meeting Scheduler Bot

📁 Repository Structure
📦 Meeting-Scheduler-Workflow
 ┣ 📄 README.md
 ┣ 📄 workflow.json
 ┗ 📂 assets
     ┗ 📸 workflow_screenshot.png

🛠 Customization

You can modify:

Email formatting

Filters (e.g., 1 day before, 1 hour before)

Sheet column names

HTML email templates

Schedule timing

If you want help customizing, I can edit the Code node for your exact logic.

🤝 Contributing

Pull requests are welcome!
Feel free to open issues or suggest improvements.

📜 License

MIT License — free to use and modify.
