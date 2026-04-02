# Microsoft Foundry Notes

## MS Foundry Agent Service

MS Foundry Agent Service supports 3 types of agents:
- Prompt agents: defined entirely through configuration
- Workflow agents (preview): no code required; support branching logic, HITL steps, multi-step orchestration
- Hosted agents (preview): built with your choice of framework and deployed in containers; Foundry manages the runtime, scaling and infrastructure

## Publishing to Microsoft 365 Copilot ##

- Publish your agent as an agent application in the Foundry UI
- Publish (again) to Microsoft 365 Copilot
- Choose "Individual scope" to publish for personal use; agent can be "shared" by giving others a link
- Choose "Organization scope" to publish to the entire company. The agent appears under "Built by your org" in the agent store [requires admin approval]
- 