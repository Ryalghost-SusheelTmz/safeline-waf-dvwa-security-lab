# Detection findings and analyst assessment

## Recorded scope

The security test was conducted against DVWA in an isolated VMware lab. The relevant SafeLine screens show traffic from the Kali client address `192.168.177.129` toward the protected hostname and DVWA paths.

## Finding 01 — XSS detection and blocking

**Evidence:** `assets/evidence/16-attack-logs.png`.

SafeLine's attack log contains reflected-XSS requests toward `/DVWA/vulnerabilities/xss_r/` with an **XSS** classification and **Blocked** action. These rows support that the WAF blocked the listed requests. They do not establish elimination of every possible XSS vector.

## Finding 02 — Command injection detected in audit mode

**Evidence:** `assets/evidence/16-attack-logs.png`.

Rows targeting `/DVWA/vulnerabilities/exec/` are classified as **Cmd Inj** and marked **Audited**. That status is important: it proves logging/classification for those requests but **not** request prevention.

## Finding 03 — WAF traffic and monitoring visibility

**Evidence:** `assets/evidence/17-attack-events.png`, `assets/evidence/18-traffic-statistics.png`, `assets/evidence/19-monitoring-dashboard.png`.

The seven-day traffic analysis shows **778 requests**, **636 page views**, **41 visitors**, **2 unique IPs**, **41 blocked requests**, **11 HTTP 4xx errors** and **52 HTTP 5xx errors**. The displayed blocked rate is **5.27%** (41 / 778). These counters belong to the displayed seven-day filter, not a single test session.

The separately captured real-time protection dashboard shows **4 blocked events in its 24-hour view**. Do not add that figure to the seven-day counter, as the windows can overlap.

## Finding 04 — Rate limiting and blacklist status

**Evidence:** `assets/evidence/12-custom-rules.png` and `assets/evidence/13-rate-limiting.png`.

The event listing documents **three requests within twenty seconds** and an action of **block one minute**. Of the visible blacklist entries, only the malicious IP intelligence group block is enabled at capture; the bespoke Kali address rule is disabled.

## Finding 05 — Policy and authentication visibility

**Evidence:** `assets/evidence/14-semantic-analysis.png` and `assets/evidence/15-auth-records.png`.

Balance Mode is selected for several semantic analysis categories. The Auth page logs successful and failed authentication outcomes with source address and timestamp.

## SOC Analyst (Level 1) response workflow

1. Identify the affected application, source IP, endpoint path, WAF decision and event time.
2. Separate **blocked** and **audited** outcomes before judging severity.
3. Check if the request belongs to the authorized test period and correlate repeated events from the same source.
4. Examine rate-limiting events, authentication attempts and policy status for context.
5. Document a preliminary disposition and escalate suspicious/unexpected traffic for further investigation.
6. Recommend tuning only after verifying legitimate traffic impact and false-positive risk.

## Scope limitations

- A security alert is not, by itself, evidence of successful exploitation or of complete protection.
- The supplied screenshots do not show full HTTP request/response captures or a comprehensive before-and-after vulnerability assessment.
- The enterprise network diagram is a design artifact. The WAF application configuration shows the actual lab proxy destination separately.
- Screenshots are historical snapshots, not current online service-health reports.
