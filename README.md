# sigint-hombre

**Author:** John Page — malvuln (aka hyp3rlinx) 

Copyright (c) 2026


**Purpose:**  
Generate **derived Suricata detection rules** from **live URLhaus threat indicators** at runtime and deploy them to the **Security Onion platform** for high-coverage real-time network monitoring.

## Current Version

**v2.0.0**  
• Major refactor  
• Improved stability  
• Actively maintained  

---

## Verification (v2 – script integrity)

- **MD5:** `f92c8100417fc5a992480486203d21f5`
- **SHA256:** `e83d49b6750c6853eac59d844712230ffa9723312ffb7354b5b7f95368b49583`

---

## Deprecated Version

⚠️ **v1 is deprecated and retained for reference only.**

---

## Verification (v1 – deprecated)

> Hashes for verifying the original (DEPRECATED) rule-generation script (`siginthombre.py`) .

- **MD5:** `2dcadececf1ac430cd2c98e323154148`
- **SHA256:** `1f32b7c2d890ed6910c716a9a976abb2701954733cd59db5a97344766f9c4691`

---

## What it does

- Pulls the **public URLhaus feed** in real time (**not mirrored or redistributed**)
- Skips:
  - Comments, empty lines, malformed URLs, and feed self-references
- Normalizes and extracts:
  - **Protocol, host, URI path, and port**
  - Deduplicates hosts into consistent rule keys
- Generates **original, derived Suricata alerts** for 3 layers:
  1. **HTTP** → host + URI path, **any port if none is specified** (max coverage)
  2. **TLS** → HTTPS detection via **SNI (`tls.sni`)** only
  3. **DNS** → domain lookup visibility via **`dns.query`**
- Deduplication logic:
  - HTTP → `(host, path)`
  - HTTPS/TLS → `host`
  - DNS → `host`
- Writes all alerts into a **single combined rule file**
- Restarts Suricata rules (`so-suricata-restart --force`)

---

## Compatibility & Threat Feed Attribution

- **Built independently by the author**
- Designed for **compatibility with the Security Onion platform**
- **Not affiliated with, endorsed by, or supported by Security Onion LLC**
- *Security Onion is a trademark of Security Onion LLC, acknowledged here for identification purposes only*
- **Threat feed source: URLhaus, a service operated by abuse.ch**
- *abuse.ch is a trademark of abuse.ch and is acknowledged as such*

---

## Rule Output (generated at runtime)

/opt/so/rules/suricata/siginthombre.rules
- Contains **only generated, derived detection logic**
- **Not a feed mirror, database, or IOC redistribution**
- File is overwritten on each execution to prevent stale indicators

---

## Result

Delivers broad network detection coverage for malicious infrastructure across:

- **HTTP requests** (with or without explicit ports)
- **TLS handshakes via SNI**
- **DNS domain lookups**

---

## License & Redistribution Notice

- **Apache License 2.0** (permissive, AS-IS, no warranty)
- Suricata rules are **original, derived content generated at runtime**
- This project **does not redistribute or archive third-party IOC databases or feeds**
- Not affiliated with abuse.ch or Security Onion LLC

---

## Deployment Notice

> Validate generated rules before production use.  
> No liability is provided. The author is not responsible for damages or misuse. Use at your own risk.

---

## Example cron usage (run once per hour)
0 * * * * /usr/local/bin/siginthombre.py

---

Built for defenders who want **fast fresh dynamic rule detection logic**

![siginthombre_poc](https://github.com/user-attachments/assets/d39977ba-6797-4a18-a62b-88e890306654)

All domains and IPs shown here are publicly shared malicious indicators consumed from third-party threat feeds to demonstrate Suricata alert detections using **siginthombre**.
Do not visit or interact with these domains or IPs outside of a controlled research or SOC testing environment.


