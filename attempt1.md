# Notes on setting up "staffing agent attempt 1"

- Create a new agent in the "Staffing Agent (TEST)" solution
- Give it the name "staffing agent attempt 1"
- schema name = "sax_attempt1"
- selected model is GPT-5 Chat by default
- uses "generative AI orchestration" by default
- Changed "Enable your agent to search all public websites." to Disabled (under "Knowledge")
- has four "Custom" topics by default: Goodbye, Greeting, Start Over and Thank you

## Knowledge

- I added a "Knowledge" item for **consultant profiles** in SharePoint at https://scottlogic.sharepoint.com/sites/staffing/Shared%20Documents/Consultant%20Profiles/
  - Selected "Agents can use this knowledge source" ... "At any time" (the default)
  - Did not mark the knowledge source as an "Official source"

- Trying to create a Knowledge connection to **Confluence**...
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

- Trying to create a Knowledge connection to **Kimble / Salesforce**
  - There is a connector available for Salesforce
  - It has no configuration, so I doubt it gives access to the data we need
  - But will try it...
  - "Agents can user this knowledge source" ... "At any time"
  - Not an "Official source"
    

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

## Create an Agent Flow to access Salesforce

The Salesforce connector supports many actions, including "ExecuteSoqlQuery".  
Actions can be added to Agent Flows, and Agent Flows can be made available as Tools in other agents.

Following the steps in the Copilot Studio docs page for [Create an agent flow as a tool](https://learn.microsoft.com/en-us/microsoft-copilot-studio/advanced-flow-create)

First, create a new Topic for "Staff Availability"...
- Go to the Topics list for the agent and Click "+ Add topic"
- Name the topic "Staff Availability"
- Provide text for "Create a topic to ..." ("Get information about a consultant's project assignments, including start and end dates, and availability for a new project assignment today or at a future date.")
- Click "Create" button
- Edit "Describe what the topic does" in the Trigger node. Add example questions the topic can answer.
- Click "Save" button
- Click the "+" below the Trigger node and choose to create a new Tool.
- Under "Basic tools" select "New Agent flow"
- Select "Publish" to save the flow before making any changes
- "Your agent flow was published" message comes up. Choose "Go back to agent".
- The new flow appears in an Action node in the topic.
- Click "Save" button to finish adding the flow as a tool.
- Click the "Flows" icon on the vertical menu on the left side of the screen.
- Open the new agent flow ("Untitled")
- Go to the Overview panel (select from the top menu)
- Click the "Edit" button (side panel opens)
- Give the flow a name ("Get Activity Assignment report data")
- Add a Description ("Send an SOQL query to get the Activity Assignment report data using the Salesforce connector")
- Set the "Primary owner" (kept the default "Robert Lewis")
- Click "Save" button (side panel closes)
- Go to the Designer panel (select from top menu)
- For now, not adding any input parameters to "When an agent calls the flow" trigger
- Click the "+" below the "When an agent calls the flow" trigger
- In the Add an action dialog, search for "Salesforce".
- Choose "Queries - SOQL Query (Preview)"
- Enter a Connection name ("Activity Assignment SOQL query")

TOTALLY WINGING IT FROM HERE, AND USING THE USER AUTH TOKEN EXTRACTED BY manage-sf-session.sh - which could be completely wrong.
(NOTE: That was completely wrong.)

Try running a "Test" of the new Flow
- Open the "Designer" panel of the "Get Activity Assignment report data" flow
- Click the "Test" icon in the top menu bar
- (Something happened here where I was asked to "publish and test" or something like that)
- The "Test Flow" side panel opens on the right
- Choose "Manually" and click "Test" button at the bottom of the side panel
- Go to the Flows page (select from the left vertical menu bar)
- Select "Get Activity Assignment report data" flow
- Select the "run" for the test in the 28-day run history on the Overview panel
- Error reported in the "Queries - SOQL Query" node: "Unauthorized"
- Click the node to show details
- Under "Outputs" the Body has field message = "Access denied due to invalid subscription key. Make sure to provide a valid key for an active subscription."

MS documentation: https://learn.microsoft.com/en-ca/connectors/apptigentcloudtools/




## Git Integration

After creating the agent, and adding it to the StaffingAgentTEST solution, it does not appear in the solution repository in Azure DevOps automatically.

To commit changes...
- Go to Source Control panel in the Solution page (look for git symbol at the bottom of the left side vertical menu bar)
- Uncommitted changes are displayed
- Click "Commit" in the top menu
