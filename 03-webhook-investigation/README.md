# Lab 3: Webhook Delivery Investigation

## Objective

Investigate a simulated webhook delivery failure involving a completed call event, identify the cause of an HTTP authentication error, and review the delivery result following a simulated credential renewal.

## Tools Used

- Windows PowerShell 5.1
- Simulated webhook delivery records
- HTTP status code analysis
- GitHub for technical documentation

## Customer Scenario

A customer reports that completed calls are not appearing in their connected CRM.

The SaaS platform generates a `call.completed` event, but the webhook delivery fails.

**Simulated event details:**

| Field | Value |
|---|---|
| Account ID | `ACCT-1042` |
| Event ID | `evt-78291` |
| Event Type | `call.completed` |
| HTTP Method | `POST` |
| Destination | `https://crm.example.com/webhooks/calls` |
| Initial Response | `401 Unauthorized` |

## Investigation

The simulated webhook response included an `invalid_token` error indicating that the authentication token had expired.

I identified authentication as the likely cause and selected the appropriate remediation: verify the token expiration, renew the credential through an approved process, and retry the failed event.

To examine delivery results, I created two structured PowerShell records representing the original failure and a subsequent successful HTTP response.

I used `Format-Table` to review both attempts and `Where-Object` to filter responses with HTTP status codes of 400 or higher.

## PowerShell Commands

```powershell
$deliveries | Format-Table -AutoSize

$deliveries |
    Where-Object { $_.StatusCode -ge 400 } |
    Format-Table -AutoSize
```

## Results

| Attempt | HTTP Status | Finding |
|---|---|---|
| 1 | `401 Unauthorized` | Expired authentication token |
| 2 | `200 OK` | Request accepted after simulated credential renewal |

The PowerShell filter correctly isolated the failed attempt.

## Root Cause and Resolution

**Root cause:** The simulated receiving endpoint rejected an expired authentication token.

**Recommended resolution:** Verify and renew the affected credential, retry the failed webhook event, and confirm that the destination accepts the request.

**Additional verification:** Check that the corresponding call record appears in the CRM. An HTTP `200 OK` response alone does not establish that the CRM record was created.

## Screenshot

![PowerShell webhook delivery investigation](webhook-delivery-investigation.png)

## Key Takeaways

- Correlating event IDs and customer account IDs helps isolate integration issues.
- HTTP `401 Unauthorized` responses can indicate invalid or expired credentials.
- PowerShell can filter structured delivery records to identify failures.
- Successful HTTP delivery and successful downstream CRM synchronization are separate verification steps.

**Lab note:** This exercise used fictional delivery records. No live webhook request, credential renewal, or CRM synchronization was performed.

