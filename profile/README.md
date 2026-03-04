<p align="center">
  <a href="https://agentphone.app">
    <img src="https://raw.githubusercontent.com/agentphone-app/.github/main/profile/logo.svg" width="80" alt="AgentPhone" />
  </a>
</p>

<h1 align="center">AgentPhone</h1>


https://github.com/user-attachments/assets/3acc7ab2-e84d-46b9-89d8-abba43515c6d


<p align="center">
  <strong>Phone calls for AI agents.</strong><br>
  Book reservations, cancel subscriptions, check claim status — with a single API call.
</p>

<p align="center">
  <a href="https://agentphone.app">Website</a> · <a href="https://agentphone.app/docs">API Docs</a> · <a href="https://github.com/agentphone-app/skill">OpenClaw Skill</a>
</p>

---

### How it works

1. Your agent sends a phone number + objective to the API
2. A voice AI makes the call — navigates IVR menus, talks to humans
3. You get back a structured outcome, transcript, and recording

```bash
curl -X POST https://agentphone.app/api/v1/calls \
  -H "Content-Type: application/json" \
  -H "x-api-key: $AGENTPHONE_API_KEY" \
  -d '{
    "to_phone_number": "+14155551234",
    "objective": "Book a table for 4 tonight at 7pm under the name Sarah",
    "business_name": "Luigis Italian"
  }'
```

**5 free credits on signup.** → [agentphone.app](https://agentphone.app)
