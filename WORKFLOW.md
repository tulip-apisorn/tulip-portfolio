# Workflow draft — Document Cabinet

Draft for discussing the flow before touching the real prototype (`index.html`).
Two diagrams: how the prototype works today, and the automation target for
later.

## 1. Today — the prototype (Phase 1)

What actually happens right now when you log one document.

```mermaid
flowchart LR
    A["Capture the document<br/>done by hand"] --> B["Fill in the log form<br/>by hand, today"]
    B --> C[("Central record<br/>localStorage, one device")]
    C --> D{"Storage type?"}
    D -->|Cloud| E1["Paste the cloud link"]
    D -->|Physical| E2["Note the physical location"]
    E1 --> F["Status shown on the<br/>prototype site only"]
    E2 --> F
    F --> G["Renew / handle it<br/>in real life"]
    G -->|"mark renewed"| C
```

## 2. Future — automated (Phase 2, with Phase 3 as an optional add-on)

Same shape, but logging and reminding are no longer manual.

```mermaid
flowchart LR
    OCR["OCR / vision reads<br/>expiry date (Phase 3)"] -.optional.-> B2
    A2["Capture the document<br/>done by hand"] --> B2["Agent reads inbox/chat<br/>and logs it (Phase 2)"]
    B2 --> C2[("Central record<br/>Google Sheet, shared")]
    C2 --> D2{"Storage type?"}
    D2 -->|Cloud| E12["Cloud link stored"]
    D2 -->|Physical| E22["Physical location stored"]
    E12 --> F2["Calendar event +<br/>LINE/Telegram reminder"]
    E22 --> F2
    F2 --> G2["Renew / handle it<br/>in real life"]
    G2 -->|"mark renewed"| C2
```

## 3. What belongs to which phase

Whether to skip straight from Phase 2 to Phase 3 can be decided later — this
is just a reasonable draft order.

| Phase | Capture | Log entry | Central record | Reminder |
|---|---|---|---|---|
| **Phase 1 — now** (prototype) | Photo/kept by hand as usual | Typed into the web form by hand | localStorage, single device | Only visible on-site, no real alert |
| **Phase 2** | Same as above | Agent reads/fills in from an inbox or chat | Google Sheet via Apps Script (shareable) | Apps Script creates a Calendar event + fires LINE/Telegram ahead of time |
| **Phase 3 — stretch** | Same as above | OCR/vision reads the expiry date from the photo automatically | Same as Phase 2 | Same as Phase 2 |
