# Milestone5NextLeap
# Weekly Product Pulse + Fee Explainer (n8n + MCP)

Turns Play Store reviews of ChatGPT into a weekly product pulse and a
customer-facing fee explanation, then logs them to Google Docs and
creates a Gmail draft after human approval.

## Fee issue identified from reviews
<one or two lines: e.g. users report the free-trial payment failing
while normal purchase works, and a cheaper tier priced about 3x the
advertised USD price. Quote 1-2 review lines.>

## How to run
1. Import workflow/Milestone-5.json into n8n.
2. Connect credentials: Google Drive, Gmail, Groq (and Google Docs and
   Gmail on the MCP server workflow).
3. Upload data/reviews_sample.csv to Google Drive and select it in the
   "Download file" node.
4. Create the MCP server workflow (MCP Server Trigger + Google Docs tool
   + Gmail "Create Draft" tool), activate it, and paste its Production
   URL into both MCP Client nodes.
5. Set your Google Doc ID in the "Edit Fields1" node.
6. Click Execute workflow.

## Where MCP approval happens
The "Send message and wait for response" node emails an approval request
with the pulse and explanation. The workflow pauses until Approve or
Disapprove is clicked. Only on Approve does the If node pass data to the
MCP Client nodes, which append to the doc and create the draft. Disapprove
writes nothing. Emails are drafts only, never sent.

## Architecture
Drive CSV -> Extract -> Clean -> Aggregate -> Review Intelligence (LLM)
-> Weekly Pulse (LLM) -> Fee Explainer (LLM) -> Approval (Gmail)
-> If -> MCP Client (Google Docs append) -> MCP Client (Gmail draft)

## Outputs
See /outputs and /screenshots.

## Sources
See sources.md.

## Placeholders in the exported workflow
The JSON is shared with two placeholders for safety:

| Placeholder | Where | Replace with |
|---|---|---|
| your-email@example.com | "Send message and wait for response" node | Your email for approval requests |
| YOUR_MCP_SERVER_PRODUCTION_URL | Both MCP Client nodes | Production URL of your MCP Server Trigger workflow |

Everything else (Google Doc ID, Drive file ID) can be swapped for your own
in the "Edit Fields1" and "Download file" nodes.

A working run is shown in the demo video and screenshots.
