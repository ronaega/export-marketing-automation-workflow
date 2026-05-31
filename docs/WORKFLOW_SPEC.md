# Workflow Specification

## Command Center Routing

The command center receives chat input and maps it to one of three manually callable workflows.

| User Intent | Workflow |
| --- | --- |
| Find, search, discover, collect, or add buyers | WF-01 Buyer Discovery |
| Send company profile, catalog, or outreach email | WF-02 Email Outreach |
| Check replies or classify email responses | WF-03 Reply Processor |

Follow-up requests are not executed from chat. Follow-up automation runs on its own schedule in WF-04.

## WF-01 Buyer Discovery

Input:

```json
{
  "product": "clove",
  "country": "Turkey",
  "limit": 10
}
```

Rules:

- Require a product or industry before running.
- Default `limit` to 10 when not specified.
- Search public internet resources.
- Prioritize company name and email.
- Save `Status` as `No Action`.
- Do not ask clarification questions inside the subworkflow.

Output shape:

```json
{
  "leads": [
    {
      "Company Name": "Example Import Company",
      "Country": "Turkey",
      "Contact Person": "",
      "Email": "info@example.com",
      "Phone Number": "",
      "Website": "https://example.com",
      "Source": "https://example.com/contact",
      "Product Interest": "clove",
      "Buyer Score": 85,
      "Status": "No Action",
      "Reason": "Imports or distributes relevant product category"
    }
  ]
}
```

## WF-02 Email Outreach

Input:

```json
{
  "country": "Turkey"
}
```

Rules:

- Read leads where `Status = No Action`.
- If a country is provided, process only that country.
- Generate a concise personalized opener.
- Send the company profile and catalog.
- Update status to `Company Profile Has Been Sent`.
- Update `Last Contact Date`.

## WF-03 Reply Processor

Rules:

- Read recent unread replies.
- Extract sender email.
- Classify reply status.
- Update the buyer row by matching `Email`.

Classification labels:

- Inquiry
- Negotiation
- Purchase Order
- Rejected

## WF-04 Follow-Up Engine

Rules:

- Runs by schedule.
- Reads the buyer database.
- Selects candidates based on status and elapsed days.
- Sends a polite follow-up email.
- Updates `Last Contact Date` and `Last Follow-Up Stage`.

Recommended schedule:

- Daily at a controlled business hour.

Recommended safety:

- Keep it inactive until outreach and reply handling are tested.
