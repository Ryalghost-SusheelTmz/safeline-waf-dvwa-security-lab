# Original evidence catalog — SafeLine WAF + DVWA

All filenames refer to the user's original files in `assets/evidence/`. Captions are aligned to the order of the case study. The screenshots have **not** been synthesized or redrawn.

| # | Original evidence file in repository | Observation supported |
|---|---|---|
| 01 | `01-enterprise-architecture.jpg` | Financial-enterprise conceptual topology including DMZ/WAF |
| 02 | `02-waf-request-workflow.png` | WAF protection mechanism diagram (not runtime evidence) |
| 03 | `03-safeline-deployment.png` | SafeLine documentation and script-management terminal |
| 04 | `04-apache-ports.png` | Apache listening on 8080 |
| 05 | `05-apache-virtual-host.png` | Apache virtual host, document root, server name |
| 06 | `06-apache-enabled-config.png` | Apache enabled configs |
| 07 | `07-apache-tls-certificate.png` | SSL certificate and key paths |
| 08 | `08-ubuntu-hosts.png` | Ubuntu hostname resolution |
| 09 | `09-kali-hosts.png` | Kali hostname resolution |
| 10 | `10-dvwa-access.png` | DVWA homepage in Kali browser |
| 11 | `11-proxy-application.png` | SafeLine defense mode, 443/HTTPS, reverse proxy, HTTP 8080 upstream |
| 12 | `12-custom-rules.png` | Enabled and disabled blacklist rules |
| 13 | `13-rate-limiting.png` | Three requests/20 seconds, one-minute rate-limit action |
| 14 | `14-semantic-analysis.png` | Balance Mode for WAF semantic attack categories |
| 15 | `15-auth-records.png` | Authentication successes and failure |
| 16 | `16-attack-logs.png` | Blocked XSS and audited command-injection events |
| 17 | `17-attack-events.png` | Event groups/durations by IP |
| 18 | `18-traffic-statistics.png` | Seven-day request and block counts |
| 19 | `19-monitoring-dashboard.png` | Separate 24-hour WAF monitoring view |

## Evidence quality and privacy

The screenshots show private-network addresses, application hostnames and UI account labels. These can identify the lab setup and should be reviewed before public upload. Do not commit private keys, credentials or session tokens. A separate infographic or project poster should be labeled **presentation artwork**, never the primary test evidence.

## Reading the findings correctly

- **Enabled** and **Disabled** deny rules are different states.
- **Audited** and **Blocked** attack events represent different actions.
- **HTTPS on SafeLine port 443** does not prove HTTPS upstream to Apache.
- **Seven-day** and **24-hour** dashboard numbers must not be interpreted as the same timeframe.
