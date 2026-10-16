# Lab 3: Webhook Delivery Investigation

## Objective

Investigate a simulated failed webhook delivery associated with a completed call event. Trace the event using account identifiers, timestamps, HTTP response codes, and delivery details to determine the likely cause of failure.

## Scenario

A customer reports that completed calls are not appearing in their connected CRM, even though the calls are successfully recorded in their phone system.

The SaaS platform generates a `call.completed` event and attempts to send it to the customer's configured webhook endpoint.

The customer reports that the CRM has not received the expected call information.

## Troubleshooting Goals

- Verify that the call event was generated.
- Confirm the correct customer account and webhook destination.
- Review the event timestamp and delivery attempt.
- Interpret the HTTP response code.
- Identify the likely cause of the failed delivery.
- Recommend next steps and communicate findings to the customer.

## Lab Type

Simulated webhook delivery investigation using sample event data and troubleshooting logs.
