# Notes on setting up "staff metadata agent attempt 3"

(basic idea: upload CSV file to Dataverse and use that as a data source)

- Create a new agent in the "Staffing Agent (Test)" solution
  - Open Microsoft Copilot Studio
  - Select the "Agents" icon in the vertical menu on the left side of the page
  - Click the "+ Create blank agent" button near the top right of the page
  - Enter the name "staff metadata agent attempt 3" in the dialog that appears
  - Open the "Agent settings (Optional)" dropdown on the dialog
  - Set Language = "English (United Kingdom)"
  - Set Solution - "Staffing Agent (TEST)" - the default
  - Set Schema name = "sax_metadata3"
  - Click the "Create" button
- The Copilot Studio page for the new agent is displayed (Overview panel)
- Click the "Edit" button in the Details box
  - Set the Description to "Extracts information about Scott Logic consultants and staff from data in Dataverse (from an uploaded CSV file). Information available includes the name, job title, business unit, job function, seniority level, office location, start date and end date for all Scott Logic employees."
  - Click "Save" button for Details
- Change the selected model to Claude Opus 4.7. (Selected model is Claude Sonnet 4.6 by default.)
- Changed "Enable your agent to search all public websites." to Disabled (under "Knowledge")
- Has four "Custom" topics by default: Goodbye, Greeting, Start Over and Thank you. (Click "See all" under Topics to see them all.)
  - Click "See All" to go to the Topics tab
  - Click "System" to show the 9 System topics
  - Turn the Escalate topic "Off"
- Add an Agent Flow tool ("Today") to return today's date (no inputs; output: today=formatDateTime(utcNow(), 'dd MMMM yyyy'))
- Click "Settings" button near top right of page
- "Generative AI" panel is shown
  - Uses "generative AI orchestration" by default
  - User Feedback is turned On by default (thumbs-up or thumbs-down with an optional comment)
  - Under Knowledge, turn off "Allow ungrounded responses"
  - Under File processing capabilities, "File uploads" is On by default. Turn it Off.
  - Under File processing capabilities, "Code interpreter" is Off by default (Leave it off.)
  - "Turn on Work IQ" is turned On by default (not sure but leaving on)
  - Click "Save" button at bottom of page
  - Click the "X" in the top right corner to close the Settings panel
  - On the Overview panel, click "Edit" in the Instructions box
- Add a knowledge source for the exported Kimble consultant metadata report
  - Follow instructions in Staffing Agent README.md (https://github.com/ScottLogic/staffing-agent/blob/main/README.md) for exporting the Staff Metadata report from Kimble (as "consultant-metadata.csv")
  - Click "+ Add knowledge" button in the Knowledge section.
  - Select "Dataverse" connector type
  - Click the "create new items" link near the top of the Dataverse knowledge source dialog
  - Select "Import an Excel file or .CSV"
  - Upload consultant-metadata.csv
  - Click the "Import" button
  - Select "Properties" from the "..." menu on the Table Card that appears
  - Set the Display name to "Consultant Metadata"
  - (Plural name and Schema name update automatically)
  - Click "Save" button
  - (Edit table dialog closes.)
  - Click "Save and exit" button
  - Close the Power Apps browser tab
  - Select the "Consultant Metadata" table on the Dataverse knowledge source dialog
  - Click "Add to agent" button
- Add agent instructions
  - Click the "Edit" button on the Instructions section
  - Enter the following instructions...
```
Use information in the Consultant Metadata table in Dataverse to answer questions about Scott Logic staff. Information available includes the person's name, the city where the consultant works, their start date and end date, their job title and job function.

Interpret dates in Consultant Metadata as being in "dd/MM/yyyy" format.

When responding to questions about "consultants", do not include any staff in the "General and Admin" business unit. Unless the question specifically asks about former staff, do not include any information about staff with "End Date" before today.

Always use the Today tool to get today's date.
```
  - Click the "Save" button on the Instructions box


## Testing

While better than earlier attempts the answers to some basic questions are still weak. (e.g. When asked who the Lead Developers in Glasgow are, the response included a person who has left Scott Logic.)

Trying this...  
- Update the Description the Knowledge source to this:
```
This Knowledge source provides general information about Scott Logic staff members.
It refers to the Consultant Metadata table in Dataverse.

The "Resource Name" column contains the name of the Scott Logic consultant. The "Location: Location Name" column contains the name of the city for the office where the consultant works. The "Start Date" column contains the date the consultant started working at Scott Logic in "d/M/yyyy" format. The "End Date" column contains the last date the consultant worked at Scott Logic in "d/M/yyyy" format. If the "End Date" column is empty, the consultant currently works at Scott Logic. The "Primary Role: Role Name" column contains the consultant's job title, indicating both the job level and job function. The "Business Unit (Secondary): Business Unit Name" column gives the staff member's general job function.
```

No change, but if I explicitly specify that I only want people with no employment end date, I get the right answer.

Try adding this to the Knowledge description (after the other comment about End Date):
```
 If the "End Date" column contains a value, the consultant no longer works at Scott Logic.
 ```

This helps, but there are still some problems.

Also tried switching to Claude Opus 4.5 and Claude Opus 4.7. The Agent continued to include people who have left Scott Logic (i.e. who have an employment EndDate before today).

### Trying to get the agent to understand today's date

User Prompt: "What is today's date?"
Response: "No information was found that could help answer this."

But the System Prompt (Agent "Instructions" includes the date in a macro... "Today is {Text(Today(), "dd mmmm yyyy")}.")

Tried addding a specific Tool which returns the current date and adding to the system prompt "Get today's date from the Today tool."
Same result.

I COULD get the agent to tell me today's date with this prompt: "Use the Today tool to get today's date."
But it then failed on the follow-up question "Who are the Lead Developers in Glasgow?" - answer was "No data was found matching the criteria of Lead Developers currently working in Glasgow in the Consultant Metadata table."

Further testing with the Today tool added and a system prompt with instructions to use it gave better results.
Q: "Who are the Lead Developers in Glasgow?"
A: [correctly listed the 6 Lead Developers currently in the Glasgow office - and sometimes mentioned Nigel Percival as a former employee]

# Conclusion

After many false starts attempting to improve the responses, it has continued to be very difficult to get the agent to understand (1) how to determine today's date, (2) to exclude consultants with end dates in the past from responses to general queries about staff, and (3) to interpret dates in the Consultant Metadata table as being in "dd/MM/yyyy" format.
