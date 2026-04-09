# Notes on setting up "staff metadata agent attempt 2"

- Create a new agent in the "Staffing Agent (Test)" solution
  - Open Microsoft Copilot Studio
  - Select the "Agents" icon in the left vertical menu bar
  - Click the "+ Create blank agent" button near the top right of the page
  - Enter the name "staff metadata agent attempt 2" in the dialog that appears
  - Open the "Agent settings (Optional)" dropdown on the dialog
  - Set Language = "English (United Kingdom)"
  - Set Solution - "Staffing Agent (TEST)" - the default
  - Set Schema name = "sax_metadata2"
  - Click the "Create" button
- The Copilot Studio page for the new agent is displayed (Overview panel)
- Click the "Edit" button in the Details box
  - Set the Description to "Extracts information about Scott Logic consultants and staff from a file uploaded by the user. Information available includes the name, job title, business unit, job function, seniority level, office location, start date and end date for all Scott Logic employees."
- Selected model is GPT-5 Chat by default (leave it for now)
- Changed "Enable your agent to search all public websites." to Disabled (under "Knowledge")
- has four "Custom" topics by default: Goodbye, Greeting, Start Over and Thank you
- Click "Settings" button near top right of page
- "Generative AI" panel is shown
  - Uses "generative AI orchestration" by default
  - User Feedback is turned On by default (thumbs-up or thumbs-down with an optional comment)
  - Under Knowledge, turn off "Allow ungrounded responses"
  - Under File processing capabilities, "File uploads" is On by default. Leave it On. (We will upload the data to the agent this time.)
  - Under File processing capabilities, turn On "Code interpreter" (it's 
  Off by default)
  - "Turn on Work IQ" is turned On by default (not sure but leaving on)
  - Click "Save" button at bottom of page
  - Click the "X" in the top right corner to close the Settings panel
  - On the Overview panel, click "Edit" in the Instructions box
  - Enter the following instructions...
```
Expect the user to upload the "consultant-metadata.csv" file. If it hasn't been uploaded, ask the user to upload it.

Write a python script to extract the required information from the CSV file.

The "consultant-metadata.csv" file contains data about Scott Logic consultants.

The column names of the table are listed in the first row of the file. The "Resource Name" column contains the name of the Scott Logic consultant. The "Location: Location Name" column contains the name of the city for the office where the consultant works. The "Start Date" column contains the date the consultant started working at Scott Logic in "d/M/yyyy" format. The "End Date" column contains the last date the consultant worked at Scott Logic in "d/M/yyyy" format. If the "End Date" column is empty, the consultant currently works at Scott Logic. The "Primary Role: Role Name" column contains the consultant's job title, indicating both the job level and job function. The "Business Unit (Secondary): Business Unit Name" column gives the staff member's general job function.

When responding to questions about "consultants", do not include any staff in the "General and Admin" business unit. Unless the question specifically asks about former staff, do not include any information about staff with "End Date" before today.

Today is {Text(Today(), "dd mmmm yyyy")}.
```
  - Click the "Save" button on the Instructions box

## Testing

| query | agent reply | correct answer |
|-------|-------------|----------------|
| who are the developers in the glasgow office | no developers in Glasgow | 17 developers (inc 1 Technical Principal) |
| who are the lead developers in the glasgow office |  | 6 lead developers |
| how many senior delivery managers work at Scott Logic |  |  |
| which office does Josie Walledge work in | "office location is: consultant-metadata.csvcite1." | Bristol |
| which office does Fanis Vlachos work in |  |  |

The agent did not prompt me to upload a file.  
When I uploaded consultant-metadata.csv and asked the same question again, the agent wrote a python script and ran it.  

Answers after running python script are really bad.

[Try turning on "Allow ungrounded responses" ?]
