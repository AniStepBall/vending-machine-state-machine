# vending-machine-state-machine Job Listing RPA
A guided project portraying a vending machine as a state machine. Job Listing RPA (UiPath) is a compact, portfolio-friendly automation that showcases practical web table extraction, simple data distribution, and production-minded hygiene (arguments, selectors, and mail delivery).

Job Listing RPA (UiPath) — RemoteOK → Excel → Email
A UiPath automation that opens RemoteOK, extracts remote job listings, saves them to a dated Excel file, and emails the spreadsheet.

## What it does
•	Opens RemoteOK, extracts Job Title, Company, Posted, Link
•	Saves the results to Listed_Jobs_<YYYY-MM-DD>.xlsx
•	Emails the spreadsheet via SMTP with an HTML summary
## Design choices
•	Single-file orchestration (Main.xaml) keeps it easy to read and fork
•	Structured Data Extraction with a table-level selector for stability
•	Argument-driven configuration for URL and email settings
•	Email delivery via SMTP for tool-agnostic notification
## Resilience & maintenance
•	If the page structure changes, re-indicate the table and update extraction metadata
•	Keep the Extract activity’s selector as high-level as practical (e.g., id='jobsboard')
•	Use Orchestrator Assets for credentials and environment differences
## Security notes
•	No secrets are hardcoded—supply via arguments or Assets
•	Prefer App Passwords/modern auth for SMTP when required by your provider

Friendly scraping: The workflow uses table extraction with a light touch. Please respect site Terms of Service and robots.txt, identify your bot politely, and avoid aggressive schedules.
## Highlights
•	Extracts Job Title, Company, Posted, and Link
•	Exports to Listed_Jobs_<YYYY-MM-DD>.xlsx
•	Sends the file via SMTP with an HTML summary
•	Simple arguments for URL and email settings
•	Easy to re-indicate if the site changes
## Roadmap ideas
•	Add optional filters (e.g., keyword include/exclude)
•	Export CSV in addition to Excel
•	Add Slack/Teams notifications
•	Convert to Windows/Modern compatibility and leverage Table Extraction activity
•	Wrap with REFramework (logging, retries, queues)
________________________________________
## Project structure
.
├─ Main.xaml              # Orchestrates browser, extraction, Excel, email
├─ project.json           # UiPath metadata & dependencies
├─ README.md
└─ ABOUT.md
## Requirements
•	UiPath Studio 23.10 (Windows-Legacy compatibility for this template)
•	Dependencies (as in project.json):
o	UiPath.System.Activities 23.10.5
o	UiPath.UIAutomation.Activities 23.10.10
o	UiPath.Excel.Activities 2.22.3
o	UiPath.Mail.Activities 1.20.3
•	Chrome with UiPath extension (recommended)
________________________________________
## Getting started
1) Open in Studio
•	Clone/download the repo and open the project folder.
•	Open Main.xaml.
2) Configure Arguments (in the Arguments panel)

Argument	Type	Default	Notes
in_Url	String	https://remoteok.com/remote-jobs	Page to scrape
in_RecipientEmail	String	(required)	Where to send the Excel
in_SmtpHost	String	(required)	e.g., smtp.gmail.com
in_SmtpPort	Int32	587	Typical TLS port
in_SmtpUser	String	(required)	SMTP username/from
in_SmtpPassword	String	(required)	SecureString in the activity
in_UseTls	Boolean	True	Enable TLS/STARTTLS
Production tip: Store credentials in Orchestrator Assets (Text/Credential) and pass them into this workflow rather than hardcoding.

3) Run
Click Run. The bot will:
1.	Open Chrome and navigate to RemoteOK
2.	Extract Structured Data from the jobs table
3.	Write Listed_Jobs_<YYYY-MM-DD>.xlsx to the project folder
4.	Send an email with the file attached
________________________________________
## How it works
1.	Browser — Open Browser (Chrome) → in_Url
2.	Extraction — Extract Structured Data (selector targets id='jobsboard')
3.	Export — Write Range (Workbook) to Listed_Jobs_<date>.xlsx
4.	Email — Send SMTP Mail Message with the Excel file attached and a short HTML body
________________________________________
## Customization
•	Change the source: Point in_Url to a filtered or category page on RemoteOK.
•	Outlook instead of SMTP: Replace Send SMTP Mail Message with Send Outlook Mail Message and remove SMTP args.
•	Scheduling: Publish to Orchestrator and create a Time Trigger (e.g., daily at 07:00).
•	Resilience: If the site markup changes, right-click Extract Structured Data → Edit Extract Metadata and re-indicate the table.
________________________________________
## Troubleshooting
•	Extracted 0 rows
o	Ensure the Chrome extension is enabled and the page is fully loaded.
o	Re-indicate the table (jobs grid) and verify the selector points to jobsboard or the correct region.
o	Increase DelayBetweenPagesMS in the Extract activity (site may lazy-load rows).
•	Email not sending
o	Check in_SmtpHost, in_SmtpPort, in_UseTls, and credentials.
o	Some providers require App Passwords or enabling Less Secure Apps/modern auth.
•	File path issues
o	The Excel is created in the current working directory (project folder). Confirm Studio’s project path or use a full path in the workflow if needed.
________________________________________
## Ethics & compliance
•	Respect the website’s Terms of Service and robots.txt.
•	Use a modest run schedule and identify your automation when applicable.
•	Prefer an official API if/when the site offers one.
________________________________________
## License
MIT 
________________________________________
