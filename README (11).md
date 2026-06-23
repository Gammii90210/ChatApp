# 💬 ChatApp

A real-time messaging UI built with **Angular 19**, featuring a clean three-panel layout, persistent state, and a modern WhatsApp/iMessage-inspired experience — built entirely without third-party UI libraries.

---

## Table of Contents

- [Overview](#overview)
- [Distinctive Features](#distinctive-features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [UI/UX Design](#uiux-design)
- [Component Breakdown](#component-breakdown)
- [State Management](#state-management)
- [Persistence](#persistence)
- [Roadmap](#roadmap)

---

## Overview

ChatApp is a frontend-only messaging application that simulates a complete, fully interactive chat experience. Users can send messages, manage contacts, view statuses, react with emojis, pin conversations, archive chats, and explore a full contact directory — all without a backend. State is managed with Angular Signals and persisted across sessions via `localStorage`.

---

## Distinctive Features

### 1. Three-Panel Adaptive Layout

The app uses a flex-based shell: a fixed-width **sidebar** (320 px), a **chat panel** that stretches to fill the remaining space, and an optional **right panel** (Contact Info or My Profile) that slides in without disrupting the chat. All three coexist simultaneously, as seen below with the Contact Info panel open alongside an active conversation.

```
┌──────────────┬────────────────────────────────┬────────────┐
│   Sidebar    │  Chat Header + Messages        │  Contact   │
│  (Chats /    │                                │   Info  /  │
│  Contacts /  │  Message Input Bar             │  My Profile│
│   Status)    │                                │  (300 px)  │
└──────────────┴────────────────────────────────┴────────────┘
```

---

### 2. Pinned & Recent Conversation Sections

The sidebar chat list is split into clearly labelled **PINNED** and **RECENT** sections. Pinning a conversation moves it to the top section instantly. Each row shows:

- Colour-coded avatar with contact initials
- Contact name and last message preview
- Relative or absolute timestamp
- Unread message count as a purple pill badge (clears on open)
- Online presence dot

---

### 3. Contact Info Right Panel

Clicking a contact's name in the chat header slides open a **Contact Info** panel on the right. It shows:

- Large avatar with colour coding and online status
- Email and phone number (with a graceful "Not provided" fallback)
- **Shared media** grid with count badge
- Documents section
- **Message** and **Block** action buttons

Available for every contact — including those just created — without navigating away from the conversation.

---

### 4. My Profile Panel

The **"You"** avatar anchored to the sidebar footer is a clickable shortcut to the user's own profile. The panel displays username, phone number, and active status. An **Edit** button in the panel header enables inline editing of your name, phone, and status — no separate settings page required.

---

### 5. Three-Dot (⋮) Dropdown Menu

The **⋮** button in the chat header opens a polished dropdown with six contextual actions:

| Action | Behaviour |
|---|---|
| Contact info | Opens the Contact Info right panel |
| Pin chat / Unpin chat | Toggles pinned state; label updates dynamically |
| Archive chat / Unarchive chat | Toggles archived state |
| Add to group | Stub with toast notification |
| Block contact | Destructive, visually separated by a divider |
| Delete chat | Styled in red |

The menu closes automatically on any outside click.

---

### 6. Add Contact Modal — Two Modes

The modal uses a segmented toggle to switch between two distinct flows:

**From directory** — Search the pre-populated org directory by name or email. Contacts already added show a green **✓ Added** badge and become non-selectable.

**New contact** — A three-field form (Name, Email, optional Phone) with an **Add contact & start chat** CTA that creates the contact and immediately opens a conversation with them.

---

### 7. Typing Indicator

When a contact is actively composing, the chat header subtitle switches to an animated **"typing..."** label, and the message thread shows a three-dot bounce bubble at the bottom of the conversation.

---

### 8. Emoji Reactions

Each message bubble supports emoji reactions. Tapping the reaction picker toggles the emoji and updates the count in real time — no page reload or server round-trip required.

---

### 9. Read Receipts

Sent messages display a **double-check (✔✔)** icon. The icon lights up in the accent colour once the message has been read, matching the UX pattern from WhatsApp and iMessage.

---

### 10. Status Tab

A dedicated **Status** tab (separate from Chats and Contacts) provides:

- A prominent "Add status update" row for the current user
- A **Recent Updates** list with each contact's avatar and timestamp
- The ability to post and delete status updates

---

### 11. Persistent State Across Sessions

All conversations, contacts, pinned/archived states, and statuses survive a full browser refresh. Angular's `effect()` API writes to `localStorage` automatically whenever any signal changes — no explicit save calls are needed anywhere in the codebase.

---

### 12. Zero Third-Party UI Dependencies

Every component — modals, dropdowns, context menus, toasts, emoji pickers, avatar colours, status badges — is hand-rolled in Angular + SCSS. No Angular Material, PrimeNG, or any other component library is used.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Framework | Angular 19 (standalone components) |
| Language | TypeScript 5.7 |
| Styling | SCSS — component-scoped + global design tokens |
| Reactivity | Angular Signals + `computed()` + `effect()` |
| Persistence | `localStorage` via `StorageService` |
| Forms | `FormsModule` (`ngModel`) |
| Build | Angular CLI (`ng build`) |
| Testing | Karma + Jasmine |

---

## Project Structure

```
src/
├── app/
│   ├── components/
│   │   ├── sidebar/                  # Tab bar, search, conversation list, footer avatar
│   │   ├── conversation-item/        # Single row: avatar, name, preview, timestamp, badge
│   │   ├── chat-header/              # Contact name (→ info panel), status, ⋮ dropdown
│   │   ├── message-list/             # Thread: day dividers, bubbles, reactions, typing
│   │   ├── message-input/            # Compose bar: attach, image, emoji, mic, send
│   │   ├── contact-info-panel/       # Right panel: details, media, docs, action buttons
│   │   ├── my-profile-panel/         # Right panel: your profile + inline edit form
│   │   ├── contacts-panel/           # Contacts tab: list + per-row info (ⓘ) button
│   │   ├── status-panel/             # Status tab: add / view / delete updates
│   │   ├── add-contact-modal/        # Segmented modal: directory search + new contact form
│   │   ├── context-menu/             # Right-click floating overlay menu
│   │   └── toast/                    # 2.2 s transient notification banner
│   ├── services/
│   │   ├── chat.service.ts           # All state signals, computed views, and actions
│   │   └── storage.service.ts        # Typed localStorage read/write wrapper
│   ├── models/
│   │   └── chat.models.ts            # TypeScript interfaces and type aliases
│   ├── app.component.*               # Root shell — three-panel flex layout
│   └── app.config.ts                 # Angular standalone bootstrapping
├── styles.scss                       # Global CSS tokens, reset, dark mode
└── index.html
```

---

## Getting Started

**Prerequisites:** Node.js 18+ and npm.

```bash
# Install dependencies
npm install

# Start the dev server
npm start
# → http://localhost:4200

# Build for production
npm run build
```

---

## UI/UX Design

### Design Philosophy

The interface follows a **calm, utility-first aesthetic** — minimal chrome, consistent spacing, and a colour system that stays out of the way of the content. Every colour, spacing value, and interaction pattern is derived from a single token set in `styles.scss`.

### Design Tokens

```scss
--accent:           #5b50d6   /* purple — primary interactive colour       */
--accent-subtle:    #eeedfe   /* tinted backgrounds, badges, icon wells    */
--online:           #1d9e75   /* green presence indicator                  */
--surface-primary:  #ffffff   /* page and panel backgrounds                */
--surface-secondary:#f5f4f0   /* sidebar and right-panel background        */
--surface-hover:    #f0eeeb   /* list row hover state                      */
--surface-active:   #ebe9e4   /* selected / active row                     */
--text-primary:     #1a1a1a
--text-secondary:   #4a4a4a
--text-muted:       #9a9898
--border:           rgba(0,0,0,0.08)
```

Dark mode values are declared in a `@media (prefers-color-scheme: dark)` block — no JavaScript toggle required.

### Colour Avatars

Contacts are assigned one of seven avatar colour names: `teal`, `blue`, `coral`, `purple`, `amber`, `green`, `pink`. These are resolved to background/text pairs purely in CSS via `[data-color="teal"]` attribute selectors — no avatar colour logic lives in TypeScript.

### Presence Indicators

A small dot anchored to the avatar corner communicates online status across every surface — sidebar rows, chat header, contact info panel, contacts list, and the add contact modal:

- `online` → green (`--online`)
- `away` → amber
- `offline` → muted grey

---

## Component Breakdown

| Component | Responsibility |
|---|---|
| `AppComponent` | Root shell; flex layout hosting sidebar, chat, and optional right panel |
| `SidebarComponent` | Tab bar, unified search, conversation list, "You" footer avatar |
| `ConversationItemComponent` | Single row: avatar, name, message preview, timestamp, unread badge |
| `ChatHeaderComponent` | Clickable contact name, status, call/search icon buttons, ⋮ dropdown |
| `MessageListComponent` | Thread: day dividers, sent/received bubbles, reactions, typing indicator |
| `MessageInputComponent` | Compose bar: attach file, image, emoji popover, text input, mic, send |
| `ContactInfoPanelComponent` | Right panel: avatar hero, details, shared media grid, docs, action buttons |
| `MyProfilePanelComponent` | Right panel: your avatar, username, phone, status, inline edit form |
| `ContactsPanelComponent` | Contacts tab: list with per-row info (ⓘ) button |
| `StatusPanelComponent` | Status tab: post/view/delete status updates |
| `AddContactModalComponent` | Two-mode modal: directory search tab + new contact form tab |
| `ContextMenuComponent` | Floating right-click menu with state-aware, dynamically generated actions |
| `ToastComponent` | 2.2-second transient banner for stub-action feedback |

---

## State Management

All application state lives in `ChatService` as Angular Signals:

- `signal<T>()` — mutable state (conversations, contacts, statuses, active tab, right-panel mode)
- `computed<T>()` — derived views (pinned conversations, filtered search results, right-panel contact details)
- `effect()` — automatic `localStorage` sync whenever any signal changes

Key signals:

| Signal | Type | Purpose |
|---|---|---|
| `_conversations` | `signal<Conversation[]>` | Full conversation list |
| `activeConversationId` | `signal<string \| null>` | Which chat is currently open |
| `rightPanel` | `signal<RightPanel>` | `'none'` / `'contact-info'` / `'my-profile'` |
| `rightPanelContactId` | `signal<string \| null>` | Which contact's info to display |
| `activeTab` | `signal<SidebarTab>` | `'chats'` / `'contacts'` / `'status'` |

This approach avoids NgRx/RxJS overhead while still delivering fine-grained, reactive updates throughout the component tree.

---

## Persistence

`StorageService` wraps `localStorage` with typed `get<T>(key, fallback)` and `set<T>(key, value)` helpers. Three slices are persisted automatically via `effect()`:

- **`contacts`** — full contact directory, including newly created contacts
- **`conversations`** — all messages, pinned/archived/blocked states, and unread counts
- **`statuses`** — status updates per contact

On first load each signal is seeded from `localStorage` when a saved value exists, and from the `DEFAULT_*` seed constants defined in `ChatService` when it does not.

---

## Roadmap

- [ ] Voice & video calling (WebRTC)
- [ ] File and image attachment upload
- [ ] In-conversation message search
- [ ] Group chat creation (entry point scaffolded via "Add to group" dropdown action)
- [ ] Settings panel — notification preferences, manual dark/light toggle
- [ ] End-to-end encryption
- [ ] Push notifications (Service Worker / PWA)
- [ ] Backend / real-time sync (WebSocket or Firebase Realtime Database)
