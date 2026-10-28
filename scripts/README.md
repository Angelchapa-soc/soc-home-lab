# Scripts

## alert_report.py — Daily SOC alert report
Turns a SIEM alert export (CSV) into an Excel report with a summary by **severity**, **MITRE technique** and **host**, and can email it to the team.

```bash
pip install -r requirements.txt
python alert_report.py sample_alerts.csv            # build report
python alert_report.py sample_alerts.csv --email    # build and email
```

**Input columns:** `time, host, rule, technique, severity` (extra columns are kept in the detail sheet).

**Email:** set `SMTP_HOST`, `SMTP_PORT`, `SMTP_USER`, `SMTP_PASSWORD`, `REPORT_TO` as environment variables. Credentials are never stored in code.

`sample_alerts.csv` contains fictional lab alerts for testing.
