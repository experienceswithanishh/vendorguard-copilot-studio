# VendorGuard 🛡️
### Autonomous Vendor Contract Compliance System
Built on Microsoft Copilot Studio | Operative Track — Microsoft Agent Academy Hackathon 2026

---

## 📋 What is VendorGuard?

VendorGuard is a fully autonomous multi-agent vendor contract compliance system built for procurement teams.

When a vendor contract arrives by email as a PDF, VendorGuard automatically:
- Detects the email and extracts the PDF
- Creates a contract record in Microsoft Dataverse
- Extracts readable text from the PDF using AI
- Evaluates the contract against 15 compliance rules across 5 dimensions
- Scores every clause Red 🔴, Amber 🟡, or Green 🟢
- Generates a full compliance review report
- Notifies the procurement team in Microsoft Teams
- Makes all contracts queryable in natural language

**From email to compliance report in under 2 minutes. Zero manual effort.**

---

## 🎯 The Problem It Solves

Procurement teams receive vendor contracts by email and currently review them manually — reading PDFs, checking clauses against policy, writing reports, and notifying stakeholders. This takes 2-4 hours per contract and frequently misses critical clauses.

VendorGuard eliminates this entirely.

---

## 🏗️ Architecture

### Hub & Spoke Multi-Agent System
Email arrives with PDF
↓
Power Automate Trigger

Detects PDF attachment
Creates Dataverse contract record
Extracts PDF text via AI prompt
Posts Teams notification
Sends prompt to Orchestrator
↓
VendorGuard Orchestrator (Copilot Studio)
↓
┌───────────────────────────────────────┐
│  Contract      │  Compliance   │  Report &    │  Contract  │
│  Intake Agent  │  Scoring      │  Notification│  Q&A Agent │
│                │  Agent + MCP  │  Agent + MCP │            │
└───────────────────────────────────────┘
↓
Microsoft Dataverse (5 custom tables)
vg_contract
vg_vendor
vg_compliancerule (15 rules — the rulebook)
vg_complianceresult
vg_reviewreport


### Key Components

| Component | Technology |
|---|---|
| Multi-agent orchestration | Microsoft Copilot Studio |
| Data layer | Microsoft Dataverse |
| Automation | Power Automate |
| Notifications | Microsoft Teams |
| Intelligent data operations | Microsoft Dataverse MCP Server |
| PDF text extraction | AI Builder Prompts (GPT-4.1 mini) |

---

## 🤖 The Agents

### VendorGuard Orchestrator
Central coordinator. Receives all inputs — user chat and autonomous email trigger. Delegates to specialist agents using generative orchestration.

### Contract Intake Agent
Collects contract details via conversation or autonomous trigger. Calls Power Automate agent flow to create Dataverse records.

### Compliance Scoring Agent
Retrieves contract text from Dataverse. Retrieves 15 active compliance rules. Evaluates each rule. Saves Red/Amber/Green results using Microsoft Dataverse MCP Server.

### Report & Notification Agent
Reads compliance results from Dataverse. Generates review report record. Posts Teams notification. Updates contract status.

### Contract Q&A Agent
Answers natural language queries about any contract, compliance result, or review report stored in Dataverse.

---

## 📊 Compliance Dimensions

VendorGuard evaluates contracts across 5 dimensions with 15 rules:

| Dimension | Rules | Examples |
|---|---|---|
| 🟡 Commercial | 3 | Payment terms, pricing clarity, delivery terms |
| ⚖️ Legal | 3 | Liability cap, termination clause, IP ownership |
| 🔒 Data Privacy | 3 | Data protection, breach notification, data residency |
| 📊 SLA & Performance | 3 | Uptime guarantee, response times, penalty clause |
| 🏛️ Regulatory | 3 | Governing law, audit rights, subcontractor disclosure |

---

## 🚀 How to Set Up

### Prerequisites
- Microsoft Power Platform Developer environment
- Microsoft Copilot Studio license
- Microsoft 365 account with Teams
- Outlook email account

### Step 1 — Create Dataverse Tables
Create these 5 tables in your Power Platform solution:

| Table | Schema Name |
|---|---|
| Contract | vg_contract |
| Vendor | vg_vendor |
| Compliance Rule | vg_compliancerule |
| Compliance Result | vg_complianceresult |
| Review Report | vg_reviewreport |

See `/docs/dataverse-schema.md` for full column definitions.

### Step 2 — Load Compliance Rules
Import the compliance rules Excel file from `/data/VendorGuard_Compliance_Rules.xlsx` into the Compliance Rule table using Dataverse import.

### Step 3 — Create Copilot Studio Agents
Create these agents in Copilot Studio inside your solution:
1. VendorGuard Orchestrator
2. Contract Intake Agent
3. Compliance Scoring Agent
4. Report & Notification Agent
5. Contract Q&A Agent

See `/docs/agent-instructions.md` for full instructions for each agent.

### Step 4 — Configure Power Automate Flows
Create these flows:
1. **When a new vendor contract arrives by email** — main email trigger flow
2. **Create Contract Record** — agent flow for contract intake

See `/docs/flows.md` for full flow configuration.

### Step 5 — Connect MCP Server
Add Microsoft Dataverse MCP Server to:
- Compliance Scoring Agent
- Report & Notification Agent

Set credentials to Maker-provided credentials.

### Step 6 — Create PDF Content Extractor Prompt
Create an AI Builder prompt named PDF Content Extractor with document input. Add as a Run a Prompt node in the email trigger flow.

### Step 7 — Publish and Test
Publish all agents. Send a test email with a vendor contract PDF to trigger the full pipeline.

---

## 🧪 Testing

Send an email to your monitored inbox with:
- Subject containing "Vendor Contract"
- A PDF attachment of a vendor contract

VendorGuard will automatically process it end to end.

To test manually, open VendorGuard Orchestrator in Copilot Studio and type:
I want to submit a new vendor contract for review

---

## 🏆 Agent Academy Curriculum Alignment

| Mission | Concept | How Applied |
|---|---|---|
| Mission 01-02 | Agent creation & instructions | All 5 agents with precise step-by-step instructions |
| Mission 03 | Multi-agent Hub & Spoke | Orchestrator + 4 specialist connected agents |
| Mission 04 | Event triggers | Power Automate email trigger → autonomous pipeline |
| Mission 06 | AI Safety | Content moderation, fallback handling, scope redirection |
| Mission 07 | Multimodal prompts | PDF Content Extractor prompt in Power Automate flow |
| Mission 08 | Dataverse grounding | Live compliance rulebook grounding for scoring agent |
| Mission 09 | Document generation | Compliance review report generated and stored |
| Mission 10 | MCP Server | Dataverse MCP Server for intelligent data operations |
| Mission 11 | CSAT | End of Conversation topic with satisfaction collection |

---

## 📁 Repository Structure
vendorguard-copilot-studio/
├── README.md
├── data/
│   └── VendorGuard_Compliance_Rules.xlsx
├── docs/
│   ├── dataverse-schema.md
│   ├── agent-instructions.md
│   └── flows.md
└── assets/
└── architecture-diagram.png

---

## 🎥 Demo Video

https://drive.google.com/file/d/17-Y30hhX2ZBwbPJKSK8ul-wcw1BraLUy/view?usp=sharing

---

## 👤 Author

**Anish** — Processary Consulting
- LinkedIn: https://www.linkedin.com/in/experienceswithanish/
- GitHub: @experienceswithanishh

---

## 📄 License

This project is submitted as part of the Microsoft Agent Academy Hackathon 2026.
Built with ❤️ using Microsoft Copilot Studio, Dataverse, Power Automate, and Teams.
