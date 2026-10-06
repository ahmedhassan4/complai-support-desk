# ComplAI Support Desk

A multi-agent AI customer support workflow built with **Python, OpenAI Agents SDK, and Claude**.

The system automates routine support tickets while enforcing guardrails and routing high-risk cases to human agents.

## Architecture

```text
                    ┌─────────────────┐
                    │  Support Ticket │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │  Triage Agent   │
                    │                 │
                    │ category        │
                    │ urgency         │
                    │ sentiment       │
                    │ churn risk      │
                    │ prompt injection│
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │ Routing Rules   │
                    │   Python Logic  │
                    └───────┬─────────┘
                            │
                 ┌──────────┴──────────┐
                 │                     │
              Escalate              Routine
                 │                     │
                 ▼                     ▼
          Human Review       ┌─────────────────┐
                             │ Parallel Writers│
                             │                 │
                             │ Empathetic      │
                             │ Concise         │
                             │ Technical       │
                             └────────┬────────┘
                                      │
                                      ▼
                             ┌─────────────────┐
                             │ Output Guardrail│
                             └────────┬────────┘
                                      │
                                      ▼
                             ┌─────────────────┐
                             │  Picker Agent   │
                             └────────┬────────┘
                                      │
                                      ▼
                             ┌─────────────────┐
                             │  Sender Agent   │
                             └────────┬────────┘
                                      │
                                      ▼
                                  Customer
```

## Key Features

- **Multi-agent orchestration** using OpenAI Agents SDK
- **Structured outputs** with Pydantic
- **Parallel response generation** with `asyncio`
- **Knowledge-base tool calling** for grounded responses
- **Output guardrails** for generated support emails
- **Human-in-the-loop escalation** for sensitive tickets
- **Prompt injection detection**
- **Customer conversation memory** with `SQLiteSession`
- **Forced tool execution** for email delivery
- **Push notifications** for urgent cases
- **Token and AI cost tracking**
- **Support automation and ROI metrics**

## Agent Workflow

### Triage

The Triage Agent classifies each ticket into structured fields:

```text
category
urgency
sentiment
repeat_contact
churn_risk
looks_like_prompt_injection
summary
```

### Response Generation

Three specialized agents generate candidate responses in parallel:

- Empathetic
- Concise
- Technical

Each writer must use the knowledge-base tool before generating a response.

### Guardrails

Generated responses are reviewed before they can proceed.

The guardrail blocks responses containing:

- Placeholders
- Unauthorized refunds, credits, or discounts
- Legal/compliance guarantees
- Unprofessional content

### Routing

Deterministic Python rules decide whether a ticket can be handled automatically.

Examples of escalation conditions:

```text
Critical incident
Security incident
Churn risk
Repeat + unhappy customer
Prompt injection
```

This keeps critical business decisions outside the LLM.

## Example

A normal billing request can follow:

```text
Ticket
  → Triage
  → Knowledge Base
  → 3 Parallel Drafts
  → Guardrails
  → Best Draft
  → Email
```

A security incident follows:

```text
Ticket
  → Triage
  → Security Incident
  → Human Escalation
  → Push Notification
  → Human Review
```

## Tech Stack

- Python
- OpenAI Agents SDK
- Anthropic Claude Haiku 4.5
- Pydantic
- asyncio
- SQLite
- Pandas
- SMTP
- Pushover
- python-dotenv

## Setup

```bash
git clone https://github.com/<username>/complai-support-desk.git
cd complai-support-desk

pip install openai-agents anthropic pydantic pandas requests python-dotenv
```

Create a `.env` file:

```env
ANTHROPIC_API_KEY=your_api_key

# Optional: Agents SDK tracing
OPENAI_API_KEY=your_api_key

# Optional email configuration
EMAIL_ADDRESS=your_email
EMAIL_SMTP_SERVER=your_smtp_server
EMAIL_APP_PASSWORD=your_app_password
EMAIL_ADDRESS_TO=your_test_email

# Optional Pushover configuration
PUSHOVER_USER=your_user
PUSHOVER_TOKEN=your_token
```

Run the notebook:

```text
Customer-Care.ipynb
```

## Demo Metrics

The included demo processes **8 sample tickets** and demonstrates both automated and human-assisted workflows.

Example results:

| Metric | Result |
|---|---:|
| Tickets processed | 8 |
| Automated responses | 50% |
| Human escalations | 4 |
| Guardrail-blocked drafts | 5 |
| Estimated human time saved | 1.2 hrs |
| Estimated AI cost | ~$0.11 |

> Metrics are based on the included demo data and configured cost assumptions.

## Engineering Focus

This project demonstrates a controlled approach to AI automation:

**LLMs handle language understanding and generation.  
Deterministic code handles business-critical routing.  
Guardrails control generated output.  
Humans handle high-risk decisions.**

## Project Status

This is a **portfolio / educational implementation** demonstrating multi-agent support automation patterns. The knowledge base, ticket data, and business metrics are intentionally simplified for demonstration.

---
**Built with Python + OpenAI Agents SDK + Claude**