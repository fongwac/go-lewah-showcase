# Architecture (high level)

This page describes Go-Lewah's architecture at a conceptual level only. It deliberately omits hosting providers,
server names, addresses, ports, file locations and configuration.

```mermaid
flowchart LR
  subgraph Clients["People and devices"]
    A["School administrators<br/>(desktop / tablet)"]
    T["Teachers"]
    S["Students"]
    P["Parents"]
    V["Approved vendors"]
    HQ["Platform (HQ) team"]
  end

  subgraph App["Go-Lewah web application"]
    UI["React single-page app<br/>desktop and mobile layouts<br/>English / French"]
  end

  subgraph Server["Go-Lewah API"]
    API["Node.js / Express REST API"]
    AUTH["Authentication and<br/>role-based access control"]
    RULES["Business rules<br/>(school isolation, consent,<br/>approvals, audit)"]
    NOTIF["Notifications<br/>(EN / FR)"]
  end

  DB[("SQLite database")]
  MEDIA["LiveKit media server<br/>(WebRTC live audio)"]

  A & T & S & P & V & HQ --> UI
  UI -- "HTTPS / JSON" --> API
  API --> AUTH --> RULES --> DB
  RULES --> NOTIF
  API -- "short-lived, permission-scoped<br/>join tokens; room control" --> MEDIA
  UI -- "live audio (WebRTC)" --> MEDIA
  MEDIA -- "signed events<br/>(join, publish)" --> API
```

## Key design principles

- **The server decides.** Every permission is checked by the API; the app only shows or hides controls.
  Broadcasting rights are enforced by the media server itself. For example, a muted speaker's right to publish
  audio is withdrawn there, not just shown as muted in the app.
- **School isolation.** Each school sees only its own people, programmes and records.
- **Least privilege and expiry.** Radio roles belong to one programme and end with it. Join tokens are short-lived,
  and administrators delegate duties one by one.
- **Child protection.** A student can only speak on the radio with recorded parental permission. Vendors never hear
  student voices or names.
- **Accountability.** Significant actions are written to an append-only audit history.
- **Bilingual by design.** Screens, notifications and server messages are available in English and French.
- **Safe environments.** Development uses synthetic data and separate test databases. Production changes need
  separate approval.
