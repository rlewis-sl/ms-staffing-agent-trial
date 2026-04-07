# Notes on setting up "staffing agent attempt 2"

- Create a new agent in the "Staffing Agent (Test)" solution
  - Open Microsoft Copilot Studio
  - Select the "Agents" icon in the left vertical menu bar
  - Click the "+ Create blank agent" button near the top right of the page
  - Enter the name "staffing agent attempt 2" in the dialog that appears
  - Open the "Agent settings (Optional)" dropdown on the dialog
  - Set Language = "English (United Kingdom)"
  - Set Solution - "Staffing Agent (TEST)" - the default
  - Set Schema name = "sax_attempt2"
  - Click the "Create" button
- The Copilot Studio page for the new agent is displayed (Overview panel)
- Selected model is GPT-5 Chat by default (leave it for now)
- Changed "Enable your agent to search all public websites." to Disabled (under "Knowledge")
- has four "Custom" topics by default: Goodbye, Greeting, Start Over and Thank you
- Click "Settings" button near top right of page
- "Generative AI" panel is shown
  - Uses "generative AI orchestration" by default
  - User Feedback is turned On by default (thumbs-up or thumbs-down with an optional comment)
  - Under Knowledge, turned off "Allow ungrounded responses"
  - Under File processing capabilities, turned off "File uploads"
  - "Turn on Work IQ" is turned On by default (not sure but leaving on)
  - Click "Save" button at bottom of page
- Add Knowledge sources for consultant profiles
  - Go to Knowledge panel
  - Click "+ Add knowledge"
  - Select "SharePoint"
  - Enter this url in text box (with placeholder text "Enter URL of a SharePoint site"): https://scottlogic.sharepoint.com/:f:/r/sites/staffing/Shared%20Documents/Consultant%20Profiles/Bristol%20Consultant%20Profiles
  - Click "Add" button
  - The SharePoint location for "Bristol Consultant Profiles" is added
  - Add the URL for the other Scott Logic offices. Use the "Copy link" feature in each SharePoint folder to get the URLs. (The query string can be removed.)
  - Click the "Add to Agent" button at the bottom of the dialog.
  - Each of the Consultant Profile folders in added as a separate knowledge source
  - By default each of the new Knowledge sources is available "At any time". Other options are available when editing a Knowledge source under the "Include/exclude options" dropdown.
- Add knowledge sources for the exported Kimble reports
  - Follow instructions in Staffing Agent README.md (https://github.com/ScottLogic/staffing-agent/blob/main/README.md) for exporting CSV reports from Kimble.
  - Create a Knowledge source for each CSV file
    - Click "+ Add knowledge" button
    - Drag and drop the CSV file into the dialog that opens
    - Click the "Add to agent" button

(consultant-skills.csv failing after upload, so trying with a version exported in .XLSX format instead...)

## Testing

Testing approach is to use a query entered in the Claude-based version and to compare the answers.

| Query | Claude-based reply | MS-based reply |
|-------|--------------------|----------------|
| who are the standard developers in the edinburgh office | found 14, all are Edinburgh Developers | found 4, all Edinburgh, one Associate and one ex-employee |
| do we have any consultants with Azure experience | 158, 10 expert | 9 listed (TRY AGAIN AFTER consultant-skills.csv KNOWLEDGE IS READY)
| Developers currently available with CI/CD experience | 24 listed as available, based on Kimble skill level in Jenkins or Github Actions: 3 Expert, 6 Intermediate, 15 Beginner | 4 listed - only overlap is Patryk Stachowiak (BUT SKILLS CSV NOT READY) |
