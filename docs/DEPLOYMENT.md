# SafeLine WAF + DVWA — implementation record

This is a documented reconstruction from the original lab screenshots. It is specific to the configuration shown, not a generic installation script.

## Environment and network

- VMware-hosted Ubuntu web server and Kali Linux testing VM.
- Lab address shown for the web server and WAF: `192.168.177.138`.
- Kali Linux test source: `192.168.177.129`.
- Server hostname configured in both `/etc/hosts` files: `webserver.tmzsusheel`.
- A separately supplied Harbour Financial Advisory topology illustrates a broader segmented enterprise design, including a proposed DMZ. It does not prove all its VLANs were implemented for this WAF test.

## Step 1: Prepare Apache and DVWA

Inspect `/etc/apache2/ports.conf`. The observed active listener was:

```apache
Listen 8080
```

The captured `/etc/apache2/sites-available/000-default.conf` shows:

```apache
<VirtualHost *:8080>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html
    ServerName webserver.tmzsusheel
</VirtualHost>
```

These excerpts reproduce only the specific visible settings and not the full server configuration. The screenshots also show DVWA under `/var/www/html/DVWA`.

## Step 2: Review Apache TLS settings

The Apache SSL-site screenshot identifies `SSLEngine on` and the local certificate paths:

```apache
SSLCertificateFile /etc/ssl/dvwa/dvwa.crt
SSLCertificateKeyFile /etc/ssl/dvwa/dvwa.key
```

**Interpretation:** These are Apache configuration details. The WAF application is separately shown using **HTTPS 443** and **HTTP upstream 8080**; the supplied WAF screenshot does not establish TLS on the upstream link.

## Step 3: Resolve the test hostname

On both systems, the `/etc/hosts` screenshots show:

```text
192.168.177.138 webserver.tmzsusheel
```

This provided local resolution from the Kali test client and Ubuntu.

## Step 4: Deploy/manage SafeLine

The user-provided screenshot records SafeLine installation documentation alongside a terminal invoking the SafeLine management script. The captured terminal includes container stop/removal lines, so this particular frame alone cannot verify a completed installation. The later application and monitoring screenshots establish that the management UI was functioning.

## Step 5: Add a protected application

SafeLine WAF → Applications → Detail:

| Setting | Value visible in screenshot |
|---|---|
| Exposure | `443/HTTPS` |
| Access method | `Reverse Proxy` |
| Upstream | `http://192.168.177.138:8080` |
| Protection state | `Defense Mode` |

The visible web hostname in the SafeLine UI contains a slightly different spelling from the hostname in the Apache and hosts-file screenshots. Confirm canonical host spelling before recreating the setup; the screenshots are preserved unchanged.

## Step 6: Inspect defensive controls

- Custom deny rules: Malicious IP Intelligence Group Block **enabled**; JA4 fingerprint, Malicious IP and Block Kali **disabled** at capture.
- HTTP Flood → Rate Limiting: event records show a **3 requests / 20 seconds** trigger and **1-minute** access limitation.
- Semantic Analysis: Balance Mode selected for multiple categories, including XSS, SQL injection and command injection.
- Auth: records show both successful and failed password logins.

## Step 7: Inspect requests and analyze results

The original SafeLine attack logs show **Blocked** XSS requests to the DVWA reflected-XSS endpoint and **Audited** command-injection requests to the command execution endpoint. Review the event timestamps, source IP and target URL for initial SOC triage. Do not classify an audited event as prevented.

## Screenshots

The original screenshots are catalogued in [EVIDENCE.md](EVIDENCE.md) and referenced inline in the root [README.md](../README.md).
