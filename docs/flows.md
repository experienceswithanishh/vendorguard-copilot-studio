# VendorGuard — Power Automate Flows

## Flow 1 — When a new vendor contract arrives by email

### Type
Automated cloud flow (Event trigger from Copilot Studio)

### Trigger
When a new email arrives (V3) — Office 365 Outlook
- Include Attachments: Yes
- Only with Attachments: Yes
- Subject Filter: Vendor Contract

### Settings
- Split On: Enabled
- Array: `@triggerOutputs()?['body/value']`

### Flow Steps

```
1. When a new email arrives (V3)
        ↓
2. For each (attachment loop)
        ↓
3. Condition — Attachments Content-Type is equal to application/pdf
        ↓ TRUE path
4. Html to text
   - Content: Body (from trigger)
        ↓
5. Add a new row (Microsoft Dataverse)
   - Table: Contracts
   - Contract Name: Subject (from trigger)
   - Source Email: From (from trigger)
   - Contract Number: concat('CN', formatDateTime(utcNow(), 'yyMMddHHmm'))
   - Contract Status: Received
   - Upload Date: Received Time (from trigger)
   - Vendor Name: From (from trigger)
        ↓
6. Upload a file or an image (Microsoft Dataverse)
   - Table: Contracts
   - Row ID: Contract ID (from Add a new row)
   - Column name: Contract PDF
   - Content: Attachments Content (from trigger)
   - Content name: Attachments Name (from trigger)
        ↓
7. Run a prompt (AI Builder)
   - Prompt: PDF Content Extractor
   - Contract input: Attachments Content (from trigger)
        ↓
8. Update a row (Microsoft Dataverse)
   - Table: Contracts
   - Row ID: Contract ID (from Add a new row)
   - Contract Summary: Text (from Run a prompt)
        ↓
9. Post card in a chat or channel (Microsoft Teams)
   - Post as: Flow bot
   - Post in: Channel
   - Adaptive Card: VendorGuard notification card
        ↓
10. Sends a prompt to the specified copilot for processing
    - Agent: VendorGuard Orchestrator
    - Message: Contract details + autonomous processing instructions
```

### Key Design Decisions
- PDF text extraction happens in the flow (not in the agent) because document inputs are not supported for prompts added as tools in Copilot Studio agents
- Extracted text is stored permanently in vg_contractsummary so any agent can query it at any time
- Teams notification is posted from the flow for guaranteed delivery regardless of agent behaviour

---

## Flow 2 — Create Contract Record

### Type
Agent flow (called by Contract Intake Agent)

### Trigger
When an agent calls the flow

### Inputs
| Name | Type | Description |
|---|---|---|
| ContractName | Text | Name of the vendor contract |
| VendorName | Text | Name of the vendor company |
| ContractNumber | Text | Contract number in format CN##### |
| VendorCountry | Text | Country where vendor is based |

### Flow Steps

```
1. When an agent calls the flow
   Inputs: ContractName, VendorName, ContractNumber, VendorCountry
        ↓
2. Add a new row (Microsoft Dataverse)
   - Table: Contracts
   - Contract Name: ContractName (input)
   - Vendor Name: VendorName (input)
   - Contract Number: ContractNumber (input)
   - Vendor Country: VendorCountry (input)
   - Contract Status: Received
   - Upload Date: utcNow()
        ↓
3. Respond to the agent
   - Output: ConfirmationMessage (Text)
   - Value: "Contract record created successfully for " + ContractNumber
```

### Outputs
| Name | Type | Description |
|---|---|---|
| ConfirmationMessage | Text | Confirmation of successful record creation |

---

## PDF Content Extractor Prompt

### Type
AI Builder Prompt (GPT-4.1 mini)

### Input
- ContractPDF (Document/File input)

### Instructions
```
You are a document text extraction specialist.

Extract ALL readable text from the PDF document and return it as clean structured plain text.

Extract everything including:
- All clause headings and numbers
- All clause text word for word
- All tables and their content
- All dates, amounts, and figures
- All party names and details
- All schedules and annexures

Return ONLY the extracted text. No commentary or analysis.
```

### Usage
Called inside the email trigger flow as a Run a prompt node. The PDF attachment content bytes are passed as input. The extracted text output is stored in the vg_contractsummary field of the Contract record.

### Why in the flow and not as an agent tool
Microsoft Learn documents that image/document input is not yet supported for prompts added as tools in Copilot Studio agents. Therefore PDF extraction must happen inside a Power Automate flow using the Run a prompt node, which fully supports document inputs.
