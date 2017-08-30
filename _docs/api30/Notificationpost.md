---
title: Notification Post
category: API CIELO ECOMMERCE
order: 6
---

## Notification Post

The Notification Post is sent based on a selection of events to be made in the Cielo E-commerce API.

The passive notification events are:

| Payment Method          | Event                                                                    |
|-------------------------|--------------------------------------------------------------------------|
| Credit Card             | Capture                                                                  |
| Credit Card             | Cancellation                                                             |
| Credit Card             | Contact Us                                                               |
| Ticket/Boleto/Bank Slip | Conciliation                                                             |
| Ticket/Boleto/Bank Slip | Manual void                                                              |
| Electronic transfer     | Confirmed                                                                |
| Recurrence              | Disabled after reaching maximum number of attempts (Denied transactions) |
| Recurrence              | Waiting for ticket/Boleto conciliation                                   |
| Recurrence              | Restart - After payment of the ticket/Boleto                             |
| Recurrence              | Finished - "enddate" reached                                             |
| Recurrence              | Deactivation                                                             |


* **Debit Card**:  We do not notify Debit Card transactions. We suggest creating a RETURN URL, where the buyer will be sent if the transaction is completed inside the bank environment. When this URL is actived, we suggest that a `GET` should be executed, updating the transaction Status 

A `Payment Status URL` must be registered by Cielo Support team in order for the notification POST to be executed.

Characteristics of `payment Status URL`

* Must be **static**
* Limit of 255 characters.

The merchant **should** return in response to notification: **HTTP Status Code 200 OK**

If  **HTTP Status Code 200 OK** is not returned, two **more Post Notification submissions will occur**.

Exemple of Notification POST:

```json
{
   "RecurrentPaymentId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
   "PaymentId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
   "ChangeType": "2"
}
```

| Field | Description | Type | Size | Required |
| ----- | ----------- | ---- | ---------- | -------- |
| `RecurrentPaymentId` | Identifier representing the recurrence (applicable only to ChangeType 2 or 4) | GUID | 36 | No |
| `PaymentId` | Identifier that represents the transaction | GUID | 36 | Yes |
| `ChangeType` | Specifies the type of notification. See table below | Number | 1 | Yes |

<br>

| ChangeType | Description |
| ---------- | ----------- |
| 1 | Payment status change |
| 2 | Recurrence created |
| 3 | Change of Anti-Fraud Status |
| 4 | Changing recurring payment status (eg automatic deactivation) |
| 5 | Cancellation denied 