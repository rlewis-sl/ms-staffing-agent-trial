# Notes on setting up "staffing agent attempt 1"

- Create a new agent in the "Staffing Agent (TEST)" solution
- Give it the name "staffing agent attempt 1"
- schema name = "sax_attempt1"
- selected model is GPT-5 Chat by default
- uses "generative AI orchestration" by default
- Changed "Enable your agent to search all public websites." to Disabled (under "Knowledge")
- has four "Custom" topics by default: Goodbye, Greeting, Start Over and Thank you

## Knowledge

- I added a "Knowledge" item for consultant profiles in SharePoint at https://scottlogic.sharepoint.com/sites/staffing/Shared%20Documents/Consultant%20Profiles/
  - Selected "Agents can use this knowledge source" ... "At any time" (the default)
  - Did not mark the knowledge source as an "Official source"

- Trying to create a Knowledge connection to Confluence...
  - Click "+ Add knowledge" in the "Knowledge" panel on the agent Overview page/tab (https://copilotstudio.microsoft.com/environments/3a857c21-9821-eb36-8f60-419f0d9bcea2/bots/84b8d012-bc2d-f111-88b5-6045bdfbb6d1/overview?solutionId=5f3f24b8-d629-f111-88b4-6045bdfbb6d1)
  - Switch to "Advanced" connection types
  - Select "Confluence"
  - Two options for "Select Confluence connection" dialog:
    1. Created by your admin
    2. Your connections
  - Choose "Created by your admin"
  - Choose sub-option "Confluence" connector (no Description) - It's the only sub-option
  - Click "Add"
  - The new "Confluence" Knowledge item appears in the Knowledge list
  - Select the "View details" option from the ... menu
  - Under "Include/exclude options", there are two options for "Agents can use this knowledge source"
    1. "At any time" (selected by default)
    2. "Only when referenced by topics"
  - I suspect I will eventually want "Only when referenced by topics" but I'm leaving it at the default for now.
  - Note there are other (more complex) possibilities available under a "Select" dropdown for "Agents can use this knowledge source".
  - There is an option to mark the Knowledge item as an "Official Source", which apparently triggers some indicator to the user that the information is official and reliable. I did not choose that option.
    

### Abandoned Confluence "personal" connection

- Trying to create a Knowledge connection to Confluence...
  - Click "+ Add knowledge" in the "Knowledge" panel on the agent Overview page/tab (https://copilotstudio.microsoft.com/environments/3a857c21-9821-eb36-8f60-419f0d9bcea2/bots/84b8d012-bc2d-f111-88b5-6045bdfbb6d1/overview?solutionId=5f3f24b8-d629-f111-88b4-6045bdfbb6d1)
  - Switch to "Advanced" connection types
  - Select "Confluence"
  - Two options for "Select Confluence connection" dialog:
    1. Created by your admin
    2. Your connections
  - Try "Your connections"...
  - Currently displays "Not connected"
  - Click the dropdown arrow and choose "Create new connection"
  - Choose Authentication type..
  - the only option is "Log in with your Confluence account" so leave that selected
  - Click "Create" button
  - Redirects to Atlassian page "Power Platform Connectors - Confluence - Prod is requesting access to your Atlassian account."
  - There is a drop down for "Use app on" with placeholder "Choose a site". Options are:
    1. scottlogic.atlassian.net
    2. sl-staffing-assistant.atlassian.net
  - I stopped at this point without adding the knowledge source. (I didn't want the connection to have access to my private Confluence pages.)

## Git Integration

After creating the agent, and adding it to the StaffingAgentTEST solution, it does not appear in the solution repository in Azure DevOps automatically.

To commit changes...
- Go to Source Control panel in the Solution page (look for git symbol at the bottom of the left side vertical menu bar)
- Uncommitted changes are displayed
- Click "Commit" in the top menu
