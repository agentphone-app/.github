<p align="center">
  <a href="https://agentphone.app">
    <img src="https://raw.githubusercontent.com/agentphone-app/.github/main/profile/logo.svg" width="80" alt="AgentPhone" />
  </a>
</p>

<h1 align="center">AgentPhone</h1>

<p align="center">
  <strong>Phone calls for AI agents.</strong><br>
  Book reservations, cancel subscriptions, check claim status — with a single API call.
</p>

<p align="center">
  <a href="https://agentphone.app">Website</a> · <a href="https://agentphone.app/docs">API Docs</a> · <a href="https://github.com/agentphone-app/skill">OpenClaw Skill</a>
</p>

https://github.com/user-attachments/assets/3acc7ab2-e84d-46b9-89d8-abba43515c6d

---

### Get started

AgentPhone lets AI agents make real phone calls to businesses. Your agent sends a phone number and an objective ("book a table for 4 at 7pm"), and a voice AI handles the call — navigating phone menus, talking to staff, and returning a structured result with outcome, transcript, and recording.

1. **Sign up** at [agentphone.app](https://agentphone.app) — 5 free credits, no card required
2. **Install the skill** (pick one):

```bash
# OpenClaw (recommended)
clawhub install agentphone

# Manual — download SKILL.md into your project
curl -o .claude/skills/agentphone.md https://agentphone.app/skills.md
```

3. **Set your API key:**
```bash
export AGENTPHONE_API_KEY=your_key_here
```

4. **Ask your agent** to make a call — it knows how.

---

### Full API reference

Everything below is the complete API reference — for agents and developers.

#### Setup

- **Base URL:** `https://agentphone.app/api/v1`
- **Auth:** `x-api-key: $AGENTPHONE_API_KEY` header
- **Requires:** `curl`

#### When to use phone calls

Use AgentPhone when:
- The business has no API, chat widget, or online form for the task
- The task requires navigating a phone menu (IVR) or speaking to a human
- You need a verbal confirmation, appointment, or cancellation
- Online self-service is broken, gated behind login, or doesn't exist
- The user explicitly asks you to call somewhere

Do NOT use AgentPhone when:
- The task can be completed via a website, API, or email
- The phone number belongs to a private individual (not a business)
- The objective is vague or not achievable via a single phone call
- You don't have a specific phone number to call

#### Writing good objectives

Be specific. The objective is the literal instruction given to the voice agent on the call.

| Bad | Good |
|-----|------|
| "Call the restaurant" | "Ask if there's a table for 4 tonight at 7pm. If yes, book under Sarah." |
| "Cancel my subscription" | "Cancel Comcast internet for account 8834-2291. Get a confirmation number." |
| "Check on my claim" | "Call about claim #CLM-44821. Ask for current status and expected resolution date." |

Include any details the voice agent needs: account numbers, names, dates, confirmation IDs.

#### Make a call

```bash
curl -X POST https://agentphone.app/api/v1/calls \
  -H "Content-Type: application/json" \
  -H "x-api-key: $AGENTPHONE_API_KEY" \
  -d '{
    "to_phone_number": "+14155551234",
    "objective": "Book a table for 4 tonight at 7pm under the name Sarah",
    "business_name": "Luigis Italian",
    "website": "https://luigis.com"
  }'
```

**Required fields:**
- `to_phone_number` — E.164 format (e.g. +14155551234)
- `objective` — what the voice agent should accomplish

**Optional fields:**
- `business_name` — name of the business (helps the voice agent)
- `website` — URL to scrape for context before dialing

**Response:**
```json
{
  "data": { "call_id": "abc-123", "status": "queued" },
  "credits_remaining": 4
}
```

#### Poll for results

Calls take 30–180 seconds. Poll every 8 seconds until status is terminal:

```bash
curl https://agentphone.app/api/v1/calls/abc-123 \
  -H "x-api-key: $AGENTPHONE_API_KEY"
```

**Response when complete:**
```json
{
  "data": {
    "call_id": "abc-123",
    "status": "completed",
    "outcome": "achieved",
    "summary": "Booked a table for 4 at 7pm tonight under Sarah. Confirmation #R-441.",
    "transcript": "AI: Hi, I'd like to book a table...\nUser: Sure, for how many?...",
    "duration_seconds": 94,
    "recording_url": "https://..."
  }
}
```

**Terminal statuses:** `completed`, `failed`, `canceled`

#### List recent calls

```bash
curl "https://agentphone.app/api/v1/calls?limit=10" \
  -H "x-api-key: $AGENTPHONE_API_KEY"
```

#### Outcome values

- `achieved` — objective completed successfully
- `partial` — some progress, not fully resolved (check `summary`)
- `not_achieved` — objective could not be completed

#### Tips

- Always poll until you get a terminal status
- Pass `website` when available — the agent scrapes it for context before dialing
- One credit = one call. Check `credits_remaining` in the POST response
- Sign up at [agentphone.app](https://agentphone.app) for 5 free credits
