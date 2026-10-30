# CypherScan — Real-Time Threat Detection Engine

**CypherScan** is a client-side, high-throughput security analyzer that performs deterministic regular expression pattern matching against live payload streams. Designed with modern **21st.dev** design aesthetics, it provides sub-millisecond detection latency, syntax token breakdowns, defensive advisories, and an interactive audit telemetry terminal.

---

## 🚀 Key Features

* **Sub-Millisecond Evaluation**: Measures and benchmarks regex evaluation times in microseconds ($\mu s$) using the browser's high-resolution `performance.now()` API.
* **Real-Time Token Highlighting**: Identifies and visually highlights the exact threat span `[start:end]` and byte length within incoming text streams.
* **Pre-Loaded MITRE ATT&CK Signatures**: 7 enterprise-grade attack signatures ready for instant testing.
* **One-Click Test Presets**: Malicious attack samples and benign control payloads for every signature to verify detection accuracy with zero false positives.
* **Custom Rule Compiler**: Build, compile, and test your own ECMAScript regex signatures on the fly with live syntax validation.
* **Live Traffic Stream Simulator**: Ingests automated real-world HTTP queries, log entries, and payload attempts to test engine throughput.
* **Audit Telemetry Terminal**: Filter logs by severity (Alerts vs. Clean), search entries, and export forensic reports to **JSON** or **CSV**.
* **Micro-Sound Synthesizer (Web Audio API)**: Zero-dependency native synthesizer for subtle audio feedback (threat alerts, clean chimes, and UI ticks) with a quick toggle in the header.
* **100% Standalone & Client-Side**: Self-contained in a single `index.html` file. Requires zero backend, zero Node.js packages, and zero server configuration.

---

## 🛠️ How It Works

```
                                      ┌────────────────────────┐
                                      │   Live Payload Stream   │
                                      │ (HTTP, Log, Input Text)│
                                      └───────────┬────────────┘
                                                  │
                                                  ▼
                                      ┌────────────────────────┐
                                      │   CypherScan Engine    │
                                      │  (Compiled RegExp)     │
                                      └───────────┬────────────┘
                                                  │
                         ┌────────────────────────┴────────────────────────┐
                         │                                                 │
                         ▼ Match Found                                     ▼ No Match Found
              ┌──────────────────────┐                         ┌──────────────────────┐
              │ 🚨 Threat Detected   │                         │  ✅ Traffic Clear    │
              │ • Isolate & Drop     │                         │  • Forward Stream    │
              │ • Highlight Span     │                         │  • Benchmarked Pass  │
              │ • Log to Telemetry   │                         │  • Log to Telemetry  │
              └──────────────────────┘                         └──────────────────────┘
```

1. **Stream Buffer Ingestion**: When text is typed, pasted, or simulated into the stream input, the engine captures the raw buffer.
2. **Regex Execution**: The input is evaluated against the compiled regular expression of the active signature.
3. **Forensic Telemetry Extraction**:
   * Evaluates match index (`match.index`) and substring length.
   * Calculates execution duration down to $0.1\,\mu s$.
   * Extracts captured groups and highlights the dangerous token span in the stream view.
4. **Action Determination**:
   * **Threat Hit**: Generates a critical alert, triggers the audio alert tone, marks the packet as `DROP & ISOLATE`, and displays remediation best practices.
   * **Clean Pass**: Confirms the traffic as `FORWARD STREAM` with a clean verification chime.
5. **Telemetry Logging**: The event is prepended to the live feed and the terminal audit log with millisecond-accurate timestamps.

---

## 🎯 What You Can Use This For

### 1. Security Operations Center (SOC) Training & Triage
* Train tier-1 and tier-2 SOC analysts to understand how signature-based intrusion detection systems (IDS/IPS) and SIEM rules identify malicious strings.
* Inspect match offsets, byte lengths, and regex flags.

### 2. Web Application Firewall (WAF) Rule Prototyping
* Prototype and test regex rules before deploying them to production WAFs (such as Cloudflare, AWS WAF, ModSecurity, or NGINX).
* Validate whether custom patterns catch obfuscated attack variants without blocking benign user traffic.

### 3. Attack Surface & Payload Testing
* Test how specific attack payloads (SQL injection auth bypasses, stored XSS snippets, path traversals, SSRF probes) behave against pattern matchers.
* Experiment with evasion techniques (URL encoding, comment injection, whitespace alternation).

### 4. Incident Response & Log Investigation
* Paste suspicious lines from web server access logs, firewall buffers, or bash histories to verify whether they trigger specific threat indicators.
* Export matched findings directly to **JSON** or **CSV** for post-incident reporting.

---

## 🛡️ Built-In Threat Signatures

| ID | Attack Vector | MITRE ATT&CK | Description |
|---|---|---|---|
| `file` | Executable Extension Injection | **T1204** | Catches malicious binaries and script attachments (`.exe`, `.bat`, `.cmd`, `.vbs`, `.ps1`, `.sh`, `.dll`). |
| `sqli` | SQL Injection (Auth Bypass) | **T1190** | Detects classic authentication bypass tautologies (`' OR 1=1 --`) and destructive SQL DDL/DML verbs (`UNION SELECT`, `DROP TABLE`). |
| `xss` | Cross-Site Scripting (XSS) | **T1059.007** | Catches `<script>` tag injections, `javascript:` pseudo-protocol URIs, and inline event handlers (`onerror=`, `onload=`). |
| `ipv4` | IPv4 C2 / Exfiltration Endpoint | **T1071** | Validates syntactically bounded IPv4 octets (`0-255`) to identify command-and-control communication or data leaks. |
| `key` | Exposed Private Key Header | **T1552.004** | Matches standard PEM cryptographic private key headers (`RSA`, `OPENSSH`, `DSA`, `EC`, `PGP`) exposed in logs or code. |
| `traversal` | Directory Path Traversal (LFI) | **T1083** | Detects multiple parent directory climbing sequences (`../../` or `..\..\`) and URL hex-encoded bypasses (`%2e%2e/`). |
| `ssrf` | Cloud Metadata SSRF Probe | **T1557** | Flags unauthorized requests targeting AWS/Azure/GCP instance metadata endpoints (`169.254.169.254`, `metadata.google.internal`) or loopback interfaces. |

---

## 💻 How to Run

1. Open the project folder:
   ```bash
   cd "d:/TOC IA"
   ```
2. Double-click [index.html](file:///d:/TOC%20IA/index.html) or open it directly in any modern web browser (Google Chrome, Microsoft Edge, Mozilla Firefox, Apple Safari).
3. No local server, build tool, or installation is needed—it executes 100% locally in your browser.

---

## ⌨️ Keyboard Shortcuts

* **`Esc`**: Instantly clears the payload input buffer and resets the engine to standby.
* **`Ctrl + K`** (or **`Cmd + K`**): Clears the audit terminal buffer.

---

## 📄 License
Open-source under the MIT License. Feel free to customize signatures and integrate into your security workflows.
