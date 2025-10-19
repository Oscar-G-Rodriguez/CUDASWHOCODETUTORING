---
title: "CWC Tutoring Platform — Scope and Roles"
version: "1.0"
author: "Oscar Rodriguez"
date: "2025-10-18"
description: "Defines the overall scope, user roles, visibility rules, and security boundaries for the Cudas Who Code tutoring and service-hour management platform."
---

# CWC Tutoring Platform — Scope and Roles

## Purpose
This project establishes a secure, school-compliant web platform for members of **Cudas Who Code** to offer and request tutoring in programming and STEM subjects.  
It provides a structured way to pair students while protecting their privacy, enabling both parties to log verified tutoring sessions for community-service or personal records.  
Club officers will have administrative tools to moderate listings, prevent spam, and export verified service-hour reports for school use.

## System Boundaries
- **Frontend:** Next.js deployed on Vercel  
- **Backend:** Google Sheets + Google Apps Script Web App (REST endpoints)  
- **Authentication:** username + password; passwords stored as salted + peppered hashes generated in the Next.js server  
- **Hosting:** Vercel (HTTPS by default; optional custom domain)  
- **Platform:** responsive web application only — no native mobile app  

All data communication occurs through the secured Apps Script API.  
Students never access or view the underlying Google Sheet directly.

## User Roles

| Role | Description | Permissions |
|------|--------------|-------------|
| **Student** | Default user type. Can create listings, post replies, negotiate terms, accept or decline offers, and log tutoring sessions. | Full CRUD access to their own listings, offers, sessions, and personal data. |
| **Admin** | Club officers or teacher advisors. Can moderate listings, close or remove spam, verify service hours, export CSV reports, and suspend users when necessary. | Full read/write access across all tables via the admin dashboard. |
| **Suspended** | Temporary restriction for users violating platform guidelines. | Read-only; cannot post or reply. |

## Identity Visibility Rules

| Stage | Visible Name | Visible To |
|--------|---------------|------------|
| Public Listings | Public alias only | All users and visitors |
| Negotiation | Real name + alias | The two negotiating users and admins |
| Accepted Deal / Contact Page | Real name + private contact fields | The two matched users and admins |
| Admin Dashboard | Real + alias + hours data | Admins only |

Real names are collected during initial registration and remain private until a negotiation begins.  
Contact methods (e.g., Discord, Zoom link, Google Voice) are shared exclusively through the private contact page after both users verify terms.

## Safety and Compliance
- Public input fields automatically warn against sharing personal contact information.  
- All traffic is served over HTTPS.  
- The Apps Script backend requires a server-side HMAC signature; unsigned or malformed requests are rejected.  
- The Google Sheet remains private to the service account executing the script.  
- Only verified admins may view or export real names and hour totals.  
- System logs include admin moderation actions for accountability.