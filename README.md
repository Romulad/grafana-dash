grafana-dash
=============

A small perso collection of Grafana dashboard JSON models for import into Grafana.

Repository layout
-----------------

- `*.json` — Grafana dashboard JSON model files (exported from Grafana). Example: `granular-node-exporter.json`.

Purpose
-------

Keep a versioned, shareable source of Grafana dashboard JSON exports. Use these JSON files to import dashboards into a Grafana instance or to track changes to dashboards in git.

How to import a dashboard
-------------------------

1) Grafana UI

- Open Grafana -> Dashboards -> Manage -> Import.
- Upload the `.json` file or paste the JSON into the import dialog and follow the prompts.

2) Grafana HTTP API (automated)

You can import dashboards programmatically using the Grafana HTTP API. The example below posts a dashboard JSON file to Grafana and sets overwrite=true so the dashboard will be replaced if it exists.

Example (bash):

```bash
# Required: set these values
API_KEY="your_grafana_api_key"
GRAFANA_HOST="http://grafana.example.com" # include scheme and host (and port if nonstandard)
DASHBOARD_FILE="granular-node-exporter.json"

# Compact the dashboard JSON and POST it. This assumes `jq` is installed.
dashboard_json=$(jq -c . "$DASHBOARD_FILE")

curl -s -H "Content-Type: application/json" -H "Authorization: Bearer $API_KEY" \
  -X POST "$GRAFANA_HOST/api/dashboards/db" \
  -d "{\"dashboard\":$dashboard_json,\"overwrite\":true}"
```

Notes
-----

- Create an API key in Grafana (Server Admin -> API Keys) with the appropriate permissions for dashboard creation.
- If your dashboard references data sources by name, ensure the target Grafana instance has matching data sources.
- Keep exported JSON files clean (remove organization-specific IDs if you want to reuse across environments).

Contributing
------------

- Add exported dashboard JSON files at the repo root or in a subdirectory. Use clear filenames and include the Grafana version or intended audience in the filename if helpful.

License
-------

This project is released under the MIT License. See the top-level `LICENSE` file for details.