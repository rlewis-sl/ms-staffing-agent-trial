# Notes on setting up "staff metadata agent attempt 1"

- Create a new agent in the "Staffing Agent (Test)" solution
  - Open Microsoft Copilot Studio
  - Select the "Agents" icon in the left vertical menu bar
  - Click the "+ Create blank agent" button near the top right of the page
  - Enter the name "staff metadata agent attempt 1" in the dialog that appears
  - Open the "Agent settings (Optional)" dropdown on the dialog
  - Set Language = "English (United Kingdom)"
  - Set Solution - "Staffing Agent (TEST)" - the default
  - Set Schema name = "sax_metadata1"
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
  - Under File processing capabilities, turned Off "File uploads"
  - Under File processing capabilities, turned On "Code interpreter"
  - "Turn on Work IQ" is turned On by default (not sure but leaving on)
  - Click "Save" button at bottom of page
- Add a knowledge source for the exported Kimble consultant metadata report
  - Follow instructions in Staffing Agent README.md (https://github.com/ScottLogic/staffing-agent/blob/main/README.md) for exporting the Staff Metadata report from Kimble (as "consultant-metadata.csv")
  - Upload the exported CSV file ("consultant-metadata.csv") to the designated SharePoint folder (https://scottlogic.sharepoint.com/:f:/r/sites/StaffingAgent/Shared%20Documents/General/AGENT%20FILES/TEST/STAFF%20METADATA)
  - Create a Knowledge source for the designated SharePoint folder
    - Click "+ Add knowledge" button
    - Enter the URL of the designated SharePoint folder
    - Click the "Add" button
    - Change the Name to "Structured Staff Metadata"
    - Change the Description to "This knowledge source provides access to structured staff metadata."
    - Click "Add to agent" button

## Testing

| query | agent reply | correct answer |
|-------|-------------|----------------|
| who are the developers in the glasgow office | 10 listed inc. 2 leavers | 17 developers (inc 1 Technical Principal) |
| who are the lead developers in the glasgow office | 5 listed inc. 1 UX Designer | 6 lead developers |
| how many senior delivery managers work at Scott Logic |  |  |
| which office does Josie Walledge work in |  |  |
| which office does Fanis Vlachos work in |  |  |


## Results

Here is the agent prompt I used (along with some variations):

```
The "consultant-metadata.csv" file available in SharePoint contains data about Scott Logic consultants. The file can be downloaded from "https://scottlogic.sharepoint.com/:x:/r/sites/StaffingAgent/Shared%20Documents/General/AGENT%20FILES/TEST/STAFF%20METADATA/consultant-metadata.csv".

The column names of the table are listed in the first row of the file. The "Resource Name" column contains the name of the Scott Logic consultant. The "Location: Location Name" column contains the name of the city for the office where the consultant works. The "Start Date" column contains the date the consultant started working at Scott Logic in "d/M/yyyy" format. The "End Date" column contains the last date the consultant worked at Scott Logic in "d/M/yyyy" format. If the "End Date" column is empty, the consultant currently works at Scott Logic. The "Primary Role: Role Name" column contains the consultant's job title, indicating both the job level and job function. The "Business Unit (Secondary): Business Unit Name" column gives the staff member's general job function.

When responding to questions about "consultants", do not include any staff in the "General and Admin" business unit.
Unless the question specifically asks about former staff, do not include any information about staff with "End Date" before today.
```

Although the agent could answer some questions about staff metadata, it was generally poor and it was not able to "find" the CSV file to use it with a Python script.

## Try exporting data in .XLSX format

- Export Staff Metadata report from Kimble again, but this time in .xlsx format
- Upload consultant-metadata.xlsx to the designated SharePoint folder (https://scottlogic.sharepoint.com/:f:/r/sites/StaffingAgent/Shared%20Documents/General/AGENT%20FILES/TEST/STAFF%20METADATA) [we already created a Knowledge item referencing this folder]

TEST AGAIN

Getting the response "Please upload the file consultant-metadata.csv so I can filter the consultants in the Glasgow office who are developers."

Changed the agent Instructions to reference "consultant-metadata.xlsx", instead of the CSV version.

Didn't work. Same response.

Also tried changing the model to GPT-5 Reasoning. When I did that the agent stopped trying to use code interpreter, and gave answers that were as bad as ever.
