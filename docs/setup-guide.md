# Setup guide

This guide targets Wazuh 4.14.9, the latest stable release on October 26, 2026. Check the [official release notes](https://documentation.wazuh.com/current/release-notes/) before starting in case a newer stable release is available. Commands, supported operating systems, resource requirements, enrollment steps, and ports vary by release; use the documentation matching the version you install.

## 1. Plan the lab
- Choose a virtualization platform available to you.
- Create a private/host-only lab network. Avoid bridged networking unless you understand the exposure and have a specific need.
- Check the current [Quickstart system requirements](https://documentation.wazuh.com/current/quickstart.html).
- Take clean VM snapshots before configuration changes.
- Keep notes with version, date, VM roles, and sanitized network details.

## 2. Install Wazuh
Use the official [installation guide](https://documentation.wazuh.com/current/installation-guide/index.html) and one deployment method documented for your release. The quickstart is intended for a small all-in-one deployment. Follow its prerequisites and credential-handling instructions exactly. Store generated secrets securely; do not put them in this repository.

After installation, use the local/private dashboard address and confirm component health using release-specific instructions. This repository cannot determine or verify your host state.

## 3. Add one owned endpoint
Follow the current Wazuh agent deployment and enrollment steps for the endpoint OS and manager version. Confirm the agent appears connected before troubleshooting event collection. Configure only the authentication sources needed for this exercise. Avoid collecting unrelated personal data.

## 4. Validate before customizing
1. Record manager and agent versions.
2. Open **Tools → Ruleset test** in the dashboard or use `/var/ossec/bin/wazuh-logtest` on the manager.
3. Test the synthetic line from `../samples/ssh-failed-login.log`.
4. Inspect decoded fields and the matched rule. Confirm whether `srcip` is populated and which ID fires.
5. Repeat a sample in the same Logtest session if checking correlation. Counters are session-scoped; follow current docs for expected count and time window.
6. If results differ from the sample assumptions, inspect installed rules under `/var/ossec/ruleset/rules/` and the docs for that release. Adapt the parent ID/field to the actual event. Do not edit files in `/var/ossec/ruleset/`; use `/var/ossec/etc/rules/` for custom rules.

`wazuh-logtest` tests parsing and rule matching. It does not prove endpoint collection, transport, indexing, dashboard visualization, or production alert behavior end to end.

## 5. Add and review the sample rule
Read comments in `../rules/local_rules.xml`. Back up the existing local rules file before editing. Use the custom range recommended by Wazuh (100000–120000), checking for collisions. Validate XML and behavior using Logtest before restarting the manager. For live alert generation, follow the exact release documentation; a restart may be required after rule changes.

Do not run password-guessing tools or generate repeated authentication failures against public systems. If creating live events, use only a disposable account on an endpoint you own, with a few manual failed sign-ins on the isolated lab network. Stop once you have enough records, and restore temporary account/configuration afterward. Synthetic data and Logtest are sufficient for the initial exercise.

## 6. Capture portfolio evidence
Use `../screenshots/README.md`. Capture only your own lab, remove secrets and personal identifiers, and state the actual version/date. Do not claim successful setup until you have verified it yourself.
