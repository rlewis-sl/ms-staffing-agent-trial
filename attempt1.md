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

## Git Integration

- After creating the agent, and adding it to the StaffingAgentTEST solution, it **DOES NOT** appear in the solution repository in Azure DevOps.
