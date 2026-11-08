# ACR FAX Server

[日本語版はこちら](README.ja.md)

A small, fast client that speaks the HylaFAX protocol directly, so reports and images produced by **ACR (Across Report)** can go out as a fax without any extra conversion steps on your side.

> **ACR itself changes every day.** The fax pipeline is kept deliberately thin — PDF/PNG in, fax job out — so it keeps working as ACR's rendering engine grows new report layouts and output formats.

---

## How it fits in

```mermaid
flowchart TD
    A["Avalonia UI<br/>fax management · report setup · send/receive · preview"]
    B["ACR Engine<br/>report rendering"]
    C["PDF output"]
    D["PNG output"]
    E["TIFF / TIFF-F conversion<br/>resolution, bi-level, compression"]
    F["HylaFAX<br/>send queue · modem control · delivery status"]

    A --> B
    B --> C
    B --> D
    C --> E
    D --> E
    E --> F
```

`acr_fax_server` is the piece between the output of the report engine and HylaFAX: it takes a PDF or PNG, converts it to a fax-ready TIFF when needed, and submits the job over HylaFAX's own protocol — no `sendfax`/`faxstat` process wrapping involved.

| Input | Typical use |
|---|---|
| PDF → TIFF | Sending a finished report by fax |
| PNG → TIFF | Sending a screen capture / scanned image by fax |

## Status

Actively developed alongside ACR, in day-to-day internal use. Office-document conversion support is implemented upstream but intentionally left undocumented here until it's fully verified.

## Related

- **ACR (Across Report)** — the report engine whose output this server faxes
- **ACR FAX Client** — the Windows desktop app built on top of this server
