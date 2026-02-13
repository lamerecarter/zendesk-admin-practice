![Zendesk](https://img.shields.io/badge/Zendesk-Admin-blue)
![Focus](https://img.shields.io/badge/Focus-Automations%20%7C%20Messaging%20%7C%20Security-brightgreen)
![Status](https://img.shields.io/badge/Project-Active-yellow)

# LC-IT-Support | Zendesk Admin Practice Lab

> End-to-end Zendesk Admin Center build simulating a 50–99 employee internal IT support environment.


Hands-on Zendesk Admin Center build to simulate an internal IT support org (50–99 employees).  
**Focus:** ticket workflow, channels, automations, intake fields, schedules, routing readiness, and security basics.

---

## Table of Contents
- [Overview](#overview)
- [Project Goals](#project-goals)
- [Environment Summary](#environment-summary)
- [Walkthrough](#walkthrough)
  - [1) Automations](#automations)
  - [2) Email Channel Setup](#2-email-channel-setup)
  - [3) Messaging Web Widget](#3-messaging-web-widget)
  - [4) Chatbot Testing](#4-chatbot-testing)
  - [5) Ticket + Email Notifications](#5-ticket--email-notifications)
  - [6) Ticket Fields](#6-ticket-fields)
  - [7) Business Hours + Holidays](#7-business-hours--holidays)
  - [8) Agent Status + Real-Time Channels](#8-agent-status--real-time-channels)
  - [9) Security Overview](#9-security-overview)
- [Key Takeaways](#key-takeaways)
- [Next Steps](#next-steps)

<details>
  <summary><b>Step-by-step build log (click to expand)</b></summary>

- Step 1: Created automation (3-day pending reminder for Pending tickets)
- Step 2: Explored and documented email channel connection options
- Step 3: Configured Messaging web widget (renamed to Live Chat + branding update)
- Step 4: Tested chatbot with simulated customer login issue
- Step 5: Reviewed ticket creation flow + email notifications
- Step 6: Created customer-editable dropdown field (“Problem” intake field)
- Step 7: Configured business hours schedule + holiday calendar
- Step 8: Reviewed agent status behavior for real-time channels
- Step 9: Explored security overview settings (2FA, password policy, access controls)

</details>

## Overview
This repository documents my hands on practice project building a Zendesk environment from scratch to simulate a real internal IT support organization. The goal was to learn the **core admin workflows** that support real world service desks: ticket handling, customer channels, automation, business rules, forms/fields, schedules, routing readiness and security basics. I named my Zendesk instance **LC-IT-Support** and designed it as if it supported **50–99 employees**.

## Project Goals
- Learn the Zendesk Admin Center layout and where key configuration lives
- Build a basic support workflow: intake → triage → response → follow-up → closure
- Configure channels (messaging/web widget, email intake options)
- Create automation to reduce manual follow ups
- Add a customer facing dropdown field to structure intake
- Set business hours + holiday schedule
- Identify where to configure agent statuses and omnichannel routing
- Explore security configuration areas (2FA, password policy visibility, restrictions)

## Environment Summary
- **Brand/Instance:** LC-IT-Support  
- **Primary Channel:** Web Widget (Live Chat)  
- **Ticket Intake Structuring:** “Problem” dropdown (Tech support / Billing problem / Other)  
- **Hours:** Mon–Fri (Pacific Time), weekends closed  
- **Holidays:** Thanksgiving, Memorial Day, Christmas  

## Tools & Features Used

- Zendesk Admin Center
- Business Rules (Automations)
- Ticket Fields & Forms
- Messaging / Web Widget
- Schedules & Holidays
- Security Configuration

---

## Walkthrough

<a id="automations"></a>
### 1) Automations
Business Rules → Automations
I built a simple automation designed to follow up with customers who haven’t responded after a set time.

**Automation:** `Pending 3 Days Reminder to Customer`
<img width="900" height="1070" alt="Screenshot 2026-02-12 184258" src="https://github.com/user-attachments/assets/3d8682ef-b9e3-4ea4-bb6c-82aa4b966291" />


**Conditions (example from build):**
- Ticket status category = `Pending`
- Hours since update = `72` (3 days)

**Actions:**
- Send an email notification to the requester:
  - Subject: “We’re awaiting your response on ticket {{ticket.id}}”
  - Body: Friendly check-in asking if they still need help and explaining the ticket may be closed if no response
- Add a tag: `sent_reminder`

**Why this matters:**
This simulates a real IT/help desk workflow where tickets go “Pending” waiting on user input. Automated reminders prevent tickets from sitting stale and reduce agent workload.

<a id="email-channel-setup"></a>
### 2) Email Channel Setup
(Channels → Talk and email → Email)
I explored where Zendesk allows you to connect an existing support email address and confirmed the system supports multiple approaches (depending on org needs), such as:
- Email forwarding
- Microsoft Exchange connector
- Gmail connector
- Authenticated SMTP connector
- Email forwarding + authenticated SMTP
<img width="900" height="1063" alt="Screenshot 2026-02-12 184508" src="https://github.com/user-attachments/assets/fd228174-0223-41a2-af1d-aee5365d5d88" />

**Note:** I did not fully connect a mailbox in this practice step—my goal was to understand *where and how* that configuration would happen in a real implementation.

<a id="messaging-web-widget"></a>
### 3) Messaging Web Widget
(Channels → Messaging and social → Messaging)
I configured the **Web Widget** and customized the chat experience:
- Renamed channel name to: **Live Chat**
- Updated widget branding color to an **aqua blue**
- Preview-tested the widget inside Zendesk
<img width="900" height="1076" alt="Screenshot 2026-02-12 184656" src="https://github.com/user-attachments/assets/eceb1b94-7f37-43b2-9930-f8860ca3ae58" />

<a id="chatbot-testing"></a>
### 4) Chatbot Testing
(Customer Simulation)
I tested the chat experience end-to-end by submitting a realistic customer issue via the widget:

Example customer request:
> “Hi, I’m having trouble logging into my company email on my laptop. It keeps saying my password is incorrect even after I reset it. Can someone help me figure out what’s going on?”
<img width="531" height="854" alt="image" src="https://github.com/user-attachments/assets/67ef87b6-96bb-4b6f-aa19-7032b6bc0ce3" />

Zendesk responded with a connection message (“Connecting you with someone now.”), confirming the widget flow and ticket capture.

<a id="ticket-email-notifications"></a>
### 5) Ticket + Email Notifications
(Ticket Events)
I verified that Zendesk generates email notifications when tickets are created, including:
- Ticket received confirmation (unassigned vs assigned scenarios)
- Ticket metadata in the email (ticket number, requester, channel, priority, brand)
<img width="900" height="973" alt="Screenshot 2026-02-12 185332" src="https://github.com/user-attachments/assets/443d4f41-c533-4704-8b5e-d7bfe9260d5d" />

This helped me understand how ticket creation routes into the agent workflow and what notifications can look like from the customer/agent side.

<a id="ticket-fields"></a>
### 6) Ticket Fields
(Objects and rules → Tickets → Fields)
I created a **customer-editable dropdown field** to improve intake quality and categorization.

**Field type:** Drop-down  
**Display name (agent view):** Issue Type  
**Customer-facing title:** Problem  
**Customer-facing description:** “Please select the reason you’re contacting our support”  
**Permissions:** Customers can edit  
**Required:** Required to submit a request
<img width="900" height="1053" alt="Screenshot 2026-02-12 185558" src="https://github.com/user-attachments/assets/04fa5099-8b57-4e84-b582-4636572d7b86" />

**Field values added:**
- Tech support
- Billing problem
- Other

**Why this matters:**
Structured intake reduces back-and-forth and helps routing/triage (especially at scale).

<a id="business-hours-holidays"></a>
### 7) Business Hours + Holidays
(Objects and rules → Schedules)
I configured a **schedule** including:
- Time zone: **Pacific Time (US & Canada)**
- Weekly hours set for business coverage (Mon–Fri)
- Holiday schedule created, including:
  - Thanksgiving
  - Memorial Day
  - Christmas
<img width="500" height="500" alt="Screenshot 2026-02-12 185801" src="https://github.com/user-attachments/assets/2c069a14-ea38-4e13-8541-3c5a5d7712ad" />
<img width="500" height="500" alt="Screenshot 2026-02-12 185915" src="https://github.com/user-attachments/assets/0b642097-ec0c-4e98-8136-0984b8b7df05" />

**Why this matters:**
Schedules influence SLA behavior, routing expectations, and “when” automations and response targets should apply.

<a id="agent-status-real-time-channels"></a>
### 8) Agent Status + Real-Time Channels
(Workflows Planning)
Next, I began preparing to configure agent status behavior for real-time channels:
- If an agent is **Online**, they can be notified / receive chats
- If an agent is **Away/Offline**, requests should reroute to tickets or alternate queues (internally managed by Zendesk routing rules)
<img width="900" height="1056" alt="Screenshot 2026-02-12 190040" src="https://github.com/user-attachments/assets/6048bbe6-9248-4e74-91ab-4d6a8cf85779" />

This is critical for realistic live support operations, ensuring customers don’t get stuck waiting when no agents are available.

<a id="security-overview"></a>
### 9) Security Overview
(Account → Security → Security overview)
I navigated to Zendesk’s security settings to identify where key controls are managed, including:
- Agent 2FA requirements
- Password/authentication policy visibility and enforcement
- Access controls and restrictions (e.g., blocking logins from certain countries)
- Auditing and monitoring areas
<img width="900" height="1055" alt="Screenshot 2026-02-12 190124" src="https://github.com/user-attachments/assets/02514806-f8a3-4477-af33-83d756ac23df" />

For this practice environment, I kept defaults and focused on understanding where controls live and how to manage them.

## Key Takeaways
- Zendesk separates configuration into clear areas: **Channels**, **Objects & rules**, **Workspaces**, and **Security**
- Automations are powerful for reducing repetitive follow-ups and keeping queues clean
- Customer-facing fields improve intake quality and reduce resolution time
- Schedules + holidays matter for realistic service expectations and SLA alignment
- Even a basic build creates a real workflow: chat intake → ticket creation → notifications → pending → reminders → closure readiness


## Operational Impact Simulation

- Reduced stale pending tickets via 72-hour automation
- Improved intake structure with required dropdown categorization
- Configured SLA-aware business hours & holidays
- Evaluated security posture (2FA, authentication controls)



## Next Steps
- Configure omnichannel routing (queues, capacity rules, agent statuses)
- Add triggers for common workflow events (assignment, status change, escalations)
- Build views and macros for fast triage
- Create a simple help center + FAQ articles to reduce inbound tickets
- Enable 2FA for agents in a production-style configuration

---

## Why I Built This
I’m building practical, hands-on experience with service desk platforms by simulating a real company setup. This project is part of developing real operational skill—not just theoretical knowledge—so I can confidently work in IT support, help desk, or security-adjacent operations environments.
