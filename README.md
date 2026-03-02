# Printer App — Release Notes

## About

Printer App is a desktop application for print shop retailers. It connects to a central backend service, automatically receives print jobs submitted by customers, and dispatches them to the correct local printer — all without manual intervention.

The app runs in the background, polls for new jobs, downloads the print file, applies your configured printer policies, sends the job to the right printer, and reports the result back to the backend. Retailers interact with the dashboard to monitor live job status, manage printers and policies, and handle any exceptions.

---

## Features

### Authentication
- Retailer account login and signup
- JWT-based session management (persisted across restarts)

### Licence Management
- Licence key activation required before accessing the dashboard
- Licence status checked on every launch; expired or inactive licences are blocked at the gate

### Automated Job Dispatch
- Polls the backend continuously for new print jobs
- Downloads job files locally before printing (PDF and image formats supported)
- Applies printer policies to select the correct printer automatically
- Reports job status back to the backend (PENDING → PRINTING → COMPLETED / FAILED / CANCELLED)
- Handles edge cases: fast-completing jobs, spooler race conditions, duplicate claims

### Printer Management
- Lists all locally installed printers with real-time status (idle, printing, offline)
- Expandable printer rows showing the live OS print queue
- Add printers to the app's managed pool

### Printer Policies
- Configure rules per printer: colour printing, duplex, paper size
- Round-robin load balancing across printers that match the same policy
- Policy settings modal with per-printer configuration cards

### Job Queue Dashboard
- Live view of all active, completed, and failed jobs
- Job action controls (cancel, retry where applicable)
- Status badges and timestamps per job

### Printer Notifications
- Real-time notifications for print failures and system-cancelled jobs
- Notification panel accessible from the dashboard navbar

### Shop QR Code
- Generate a QR code linked to your shop's job submission page
- Display or share the QR code so customers can upload and submit print jobs from their devices

### Settings
- Profile management (account details)
- Retailer configuration

### Native Windows Printing
- Print files via Chromium's rendering pipeline → GDI → Windows print spooler
- Supports PDF and common image formats out of the box
- Printer-driver-compatible output (PCL/EMF) — no raw byte bypass issues

### Auto-Update Support
- Built-in over-the-air update infrastructure (via electron-updater)
- Update channel configurable via GitHub Releases, S3, or a generic HTTPS server

---

## Releases

---

### v1.0.0 — 2026-03-02

**Initial release.**

This is the base version of Printer App, establishing the complete core workflow from job receipt to physical print output on Windows.

#### What's included

- Full retailer authentication flow (login, signup, licence activation)
- Automated job dispatch loop: polls backend → downloads file → selects printer by policy → prints → reports status
- Windows-native printing via `webContents.print()` (Chromium → GDI → spooler) — bypasses NAPI raw-byte limitations that caused immediate job cancellation on Windows drivers
- Concurrent spooler polling to capture job IDs even for fast-completing jobs
- Printer management dashboard with live OS queue display (1-second polling)
- Printer policy engine with round-robin load balancing
- Real-time printer state monitoring via native subscription API
- Job failure notifications panel
- Shop QR code generation
- Profile and retailer settings
- MSI installer (per-user, x64)
- Auto-update infrastructure (activate by configuring the publish block in `electron-builder.yml`)

#### Platform
- Windows x64
- Installer: MSI (per-user install, no admin required)

---
