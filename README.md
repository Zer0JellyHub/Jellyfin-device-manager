# 🖥️ Jellyfin Device Manager

A custom JavaScript plugin for Jellyfin that adds a device management panel to the admin sidebar — with approval workflow, auto-blocking, grouping, and a "Waiting for Approval" screen for unauthorized devices.

---

## 📸 Screenshots

### Sidebar Button
> The device manager appears in the admin sidebar alongside other plugin entries.
<img width="1440" height="646" alt="Bildschirmfoto 2026-05-09 um 22 49 04" src="https://github.com/user-attachments/assets/85e210a1-7dae-4603-9a95-60404cdd24c0" />


---

### Device Overview
> All devices grouped by physical device + user. Unknown devices appear first.

![Device Overview](screenshots/overview.png)

---

### Approve / Reject
> Confirm or reject any device with one click. Status is saved server-side via Jellyfin's DisplayPreferences API.
<img width="1440" height="900" alt="1" src="https://github.com/user-attachments/assets/00a64bdf-699b-4461-a104-03561aed529d" />

---

### Rename a Device
> Give any device a custom name (e.g. "Albin's iPhone"). The original name stays visible below.
<img width="1440" height="900" alt="rename" src="https://github.com/user-attachments/assets/6ffd9241-4bd6-4c71-b06c-080416470ba4" />

---

### Grouping Mode
> Select multiple devices and group them under a custom name. Existing groups also get a checkbox so you can add devices directly to them.
<img width="1440" height="900" alt="Groupe" src="https://github.com/user-attachments/assets/6a7e74d6-00d6-4f8c-ae44-620c8466bb8d" />

---

### Auto-Blocking
> Enable auto-blocking to kick rejected or unknown sessions every 8 seconds. A live block log shows who got kicked and when.

---

### Waiting for Approval Screen
> Non-admin users whose device hasn't been approved yet see a fullscreen black screen instead of the Jellyfin UI. The screen disappears automatically once the admin approves them.
<img width="1290" height="2796" alt="IMG_5830" src="https://github.com/user-attachments/assets/2fcc565a-6925-496b-b15a-1e63acd97f25" />

---

### Rejected Screen
> Devices that have been explicitly rejected see a static "Access denied" message — no polling, no retry.
<img width="1290" height="2796" alt="IMG_5831" src="https://github.com/user-attachments/assets/5e061550-05fd-4dde-ab71-f8ac3a02cda2" />


---

## 🚀 Installation

1. Open your Jellyfin instance and log in as **Administrator**
2. Go to **Admin Dashboard
3. Scroll down to **" JavaScript Injector"**
4. Paste the entire contents of `jellyfin-device-manager.js`
5. Click **Save** and reload the page

> ⚠️ The sidebar button only appears for administrators.

---

## ✨ Features

| Feature | Description |
|---|---|
| 🖥️ **Sidebar Button** | Appears automatically in the admin sidebar, matches Jellyfin's native style |
| 🔴 **Badge** | Shows count of unknown/unapproved devices |
| ✅ **Approve** | Mark a device as trusted — status saved server-side |
| 🚫 **Reject** | Block a device — user sees rejection screen immediately |
| ✏️ **Rename** | Give devices custom names visible only to you |
| 📦 **Grouping** | Group multiple devices under one name (e.g. "Living Room") |
| ↩ **Ungroup** | Dissolve a group back into individual devices |
| 📦➕ **Add to Group** | In grouping mode, check a group + devices to merge them |
| 🛡️ **Auto-Blocking** | Kicks rejected/unknown sessions every 8 seconds |
| 📋 **Block Log** | Live log of blocked sessions in the side panel |
| ⏳ **Waiting Screen** | Unknown users see a black "Waiting for Approval" screen |
| 🔒 **Rejected Screen** | Rejected users see a static "Access denied" screen |
| ⌨️ **Shortcut** | `Ctrl+Shift+D` opens the device manager from anywhere |

---

## 🔧 How It Works

### Admin Side
- The script runs only for administrators (checked via `/Users/Me`)
- Device status is stored in two places:
  - **localStorage** on the admin's browser (for the UI state)
  - **Jellyfin's DisplayPreferences API** on the server (so users can read their own status)
- Blocking is done via `DELETE /Sessions/{id}` + a message notification

### User Side
- A separate script block runs for all non-admin users
- On login, it reads the user's own `DisplayPreferences` to check their `dm_status`
- **`null` / `pending`** → Waiting screen + polling every 10 seconds
- **`approved`** → Normal Jellyfin access, no screen shown
- **`rejected`** → Static "Access denied" screen, no polling

### Device Grouping Key
Devices are grouped by `DeviceName + Username` — so "Browser" used by two different users shows as two separate cards, not duplicates.

---

## 📁 File Structure

```
jellyfin-device-manager.js   ← Single file, paste into Jellyfin Custom JS
README.md                    ← This file

```

---

## 🌐 Compatibility

- Jellyfin **10.9+**
- All modern browsers (Chrome, Firefox, Safari, Edge)
- Works on any Jellyfin instance where you have admin access

---

## ⚠️ Notes

- **Grouping and aliases** are stored in the **admin's browser localStorage** — they won't sync across different admin browsers
- **Approval status** is stored on the Jellyfin server (DisplayPreferences) — visible to all admins and readable by users
- The auto-blocking runs only while the **admin's browser tab is open**
- For permanent blocking without keeping a browser tab open, a server-side Jellyfin plugin (C#) would be required

---

## 📄 License

MIT — free to use, modify, and share.
