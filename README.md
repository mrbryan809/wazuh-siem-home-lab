# Wazuh SIEM Home Lab

A three-agent Wazuh home lab for collecting and investigating authentication events. The lab uses two Windows agents and one Linux agent. Rules were tested and alerts were confirmed in the lab.

## Lab overview

- **Agents:** 3 total — 2 Windows and 1 Linux.
- **Version target for a new build:** Wazuh 4.14.9, the latest stable release on October 26, 2026. Check the [official release notes](https://documentation.wazuh.com/current/release-notes/) before installing in case a newer stable version is available.
- **Detection focus:** repeated SSH invalid-user activity, with a custom correlation rule and MITRE ATT&CK technique T1110 (Brute Force).
- **Validation:** rules were tested and alert generation was confirmed. The included synthetic SSH event supports repeatable `wazuh-logtest` validation; it is example data, not a captured lab event.
- **Safety:** this is a private home lab. Use only systems and accounts you own or are authorized to test, and do not expose Wazuh management services to the public internet.

## What is in this repository

- [`docs/architecture.md`](docs/architecture.md) — components, data flow, and lab diagram.
- [`docs/setup-guide.md`](docs/setup-guide.md) — setup and validation checkpoints.
- [`rules/local_rules.xml`](rules/local_rules.xml) — custom rule for repeated SSH invalid-user events, with version caveats.
- [`docs/investigation-walkthrough.md`](docs/investigation-walkthrough.md) — analyst workflow for reviewing authentication events.
- [`templates/incident-report.md`](templates/incident-report.md) — reusable case-report format.
- [`samples/ssh-failed-login.log`](samples/ssh-failed-login.log) — synthetic SSH event for repeatable rule testing.
- [`screenshots/README.md`](screenshots/README.md) — guidance for adding sanitized lab evidence.

## Detection and validation

The custom SSH rule correlates events that first match Wazuh SSH rule `5710`, using the same decoded source IP. The example threshold is five matching events in 120 seconds. Wazuh rule IDs, decoded fields, event formats, and correlation behavior can differ by release, so the installed ruleset and actual event are the source of truth. Validate positive and negative examples against the Wazuh version in your lab before relying on the rule.

The rule maps repeated SSH invalid-user activity to MITRE ATT&CK **T1110: Brute Force**. It is a learning detection for that event pattern, not coverage of every password failure, Windows logon failure, or brute-force technique. Wazuh also provides built-in detections; inspect their results before adding overlapping rules. No active response is configured.

## Environment and evidence

Record the exact Wazuh manager and agent versions used for your existing results, host OS, VM resources, test date, and sanitized alert output when preparing this repository for review. The 4.14.9 version target above is for a new build; it does not identify the version used for the already-tested lab. Add screenshots from the actual dashboard or Logtest session in `screenshots/`; redact usernames, IPs, tokens, and other identifying details. The files in `samples/` are synthetic and should not be presented as captured evidence.

The report file is a template. Add a completed, sanitized case write-up only when you have actual lab findings to document.

## Reproduce the lab

1. Review the [architecture](docs/architecture.md) and [setup guide](docs/setup-guide.md).
2. For a new build, use Wazuh 4.14.9 (latest stable on October 26, 2026), or check the [official release notes](https://documentation.wazuh.com/current/release-notes/) for a newer stable release. Follow the version-matched [Wazuh Quickstart](https://documentation.wazuh.com/current/quickstart.html) and [installation guide](https://documentation.wazuh.com/current/installation-guide/index.html). Keep the manager and agents on a private lab network.
3. Enroll two Windows endpoints and one Linux endpoint that you own or are authorized to monitor.
4. Test the synthetic SSH event with **Tools → Ruleset test** or `/var/ossec/bin/wazuh-logtest` on the manager. Check the decoded fields and matched IDs in your installed release.
5. Compare the results with [`rules/local_rules.xml`](rules/local_rules.xml). Back up local rules before editing and validate before restarting the manager.
6. Follow the [investigation walkthrough](docs/investigation-walkthrough.md), then document observed events in the [report template](templates/incident-report.md).

## Suggested resume entry

> Built and tested a Wazuh SIEM home lab with three agents (two Windows, one Linux); validated authentication detection rules and mapped repeated SSH invalid-user activity to MITRE ATT&CK T1110.

Use this wording only while it matches the work and evidence you can discuss. Add the repository link after publishing your sanitized evidence.

## Official references

- [Wazuh Quickstart](https://documentation.wazuh.com/current/quickstart.html)
- [Wazuh installation guide](https://documentation.wazuh.com/current/installation-guide/index.html)
- [Wazuh custom rules](https://documentation.wazuh.com/current/user-manual/ruleset/rules/custom.html)
- [Testing decoders and rules](https://documentation.wazuh.com/current/user-manual/ruleset/testing.html)
- [Wazuh MITRE ATT&CK mapping](https://documentation.wazuh.com/current/user-manual/ruleset/mitre.html)

Documentation and requirements can change. Use the documentation matching your installed release.
