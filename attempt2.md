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

(consultant-skills.csv failing after upload, so trying with a version exported in .XLSX format instead... which seems to work)

## Testing

Testing approach is to use a query entered in the Claude-based version and to compare the answers.

| Query | Claude-based reply | MS-based reply |
|-------|--------------------|----------------|
| who are the standard developers in the edinburgh office | found 14, all are Edinburgh Developers | found 4, all Edinburgh, one Associate and one ex-employee |
| do we have any consultants with Azure experience | 158, 10 expert | 6 listed (one is me and I'm sure there are better candidates) |
| Developers currently available with CI/CD experience | 24 listed as available, based on Kimble skill level in Jenkins or Github Actions: 3 Expert, 6 Intermediate, 15 Beginner | 5 listed - only overlap is Patryk Stachowiak (3 of 5 not avail) - based on consultant profiles | 
| Which consultants in Edinburgh are available and have experience with keycloak IAM, 3D visualisation or SOLR NoSQL database? | RL, AS (avail) + PT, PM (not avail) - was expecting Stu C but not incl. | RL, OG, CWM (avail) - Missed Alan S. and Stu C. |

## Focus on Availability

The agent is failing at understanding consultant availability.  
Looking at the detailed "snapshot" of a test session with the agent in Copilot Studio, it seems the agent is simply extracting blocks of text from .csv and .xlsx files. There is no sign that the agent understands the structure of the files.  
The agent needs instructions for how to exract the relevant information from the files and the ability to construct and execute relevant queries - either using a SQL interface or by executing Python scripts.  

Options for querying structured data in Copilot Studio:
- Code interpreter connected to a SharePoint Knowledge source (https://learn.microsoft.com/en-us/microsoft-copilot-studio/knowledge-code-interpreter-structured-data)
- Power Platform Connector (queries against live data). e.g. Azure Database for MySQL, Azure Cosmos DB, PostgreSQL, Salesforce.
- Add an MCP server as a Tool

