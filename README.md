# n8n MCP GitHub Assistant

A pair of n8n workflows that let you chat with an AI agent that can read your GitHub account, write to a Google Doc, and email you.

## How it works

**`mcp-server.json`: the toolbox.** An MCP Server Trigger exposes two tools to any MCP client:
- **Send email** via Gmail (to a fixed address; the AI writes the subject and body)
- **Append text** to a Google Doc

**`mcp-client.json`: the brain.** A chat interface connected to an AI agent (OpenAI gpt-5-mini) with memory and two MCP connections:
- **GitHub's official MCP server**, for reading repos, issues, and more
- **The MCP server above**, for the email and Docs tools

Example prompt: *"List my GitHub repos, add them to my doc, and email me a summary."*

## Setup

Set up the server first, then the client.

### 1. Server
1. Import `mcp-server.json` into n8n.
2. Connect **Gmail OAuth2** and **Google Docs OAuth2** credentials.
3. In the Gmail node, replace `your-email@example.com` with your address.
4. In the Google Docs node, replace `YOUR_DOC_ID` with your document's ID.
5. In the MCP Server Trigger, set **Path** to a long random string.
6. Publish the workflow and copy the **Production URL** from the trigger.

### 2. Client
1. Import `mcp-client.json` into n8n.
2. In **Own MCP Client**, paste the server's Production URL as the endpoint.
3. In **Github MCP Client**, create a Bearer Auth credential with a GitHub [fine-grained personal access token](https://github.com/settings/tokens?type=beta). Read-only access is enough for listing repos.
4. In **OpenAI Chat Model**, connect your own OpenAI credential.
5. Open the chat and try a prompt.

## Security note

The MCP server has no authentication, so its URL is the only protection. Anyone with the URL can trigger emails to your address and append text to your doc. Keep the path random and private, or enable **Bearer Auth** on the MCP Server Trigger (and the matching credential in **Own MCP Client**).
