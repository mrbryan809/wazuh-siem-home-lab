# Wazuh SIEM Home Lab

A beginner-friendly defensive security project for building a Wazuh SIEM home lab, learning how it collects authentication events, evaluates rules, and supports a basic investigation. This repository includes a setup guide and practical examples.

> **Safe-use boundary:** Keep the lab isolated and use only systems and accounts you own or are explicitly authorized to test. The examples use benign sample log lines. Do not expose the Wazuh dashboard, manager, or test endpoints to the public internet.

## What you will learn
- How the Wazuh manager, indexer, dashboard, and agent fit together.
- How to plan a small, private home lab.
- How to inspect an event, test a rule, and document findings.
- How to use an example custom rule for repeated SSH failed-login events, while checking compatibility with your installed Wazuh version.

## Repository map
- `docs/architecture.md` — components, data flow, and a diagram.
- `docs/setup-guide.md` — planning, setup checkpoints, and safe validation.
- `rules/local_rules.xml` — illustrative custom rule and deployment notes.
- `docs/investigation-walkthrough.md` — analyst workflow using sample data.
- `templates/incident-report.md` — reusable report template.
- `screenshots/README.md` — screenshot checklist and placeholders.
- `samples/ssh-failed-login.log` — synthetic example event.

## Quick start
1. Read the architecture and setup guide.
2. Choose a supported host OS and size it using the current official Wazuh Quickstart requirements. Keep the system on a private, trusted network.
3. Follow the official installation guide for your chosen Wazuh version. Record the exact version and date.
4. Enroll one endpoint you own. Start with one endpoint.
5. Before adding custom rules, use **Tools → Ruleset test** in the dashboard or `/var/ossec/bin/wazuh-logtest` on the manager with the synthetic line in `samples/ssh-failed-login.log`.
6. Compare decoded fields and matched rule ID to the assumptions in `rules/local_rules.xml`. If they differ, adapt the rule to your actual decoded fields and test again.
7. Complete the investigation walkthrough and save a sanitized report.

## Important rule caveats
The sample rule uses SSH rule `5710` as a parent and `if_matched_sid` correlation. IDs, event parsing, and fields can change between versions or differ by distribution/log format. Current Wazuh documentation shows `5710` for an SSH invalid-user event and describes built-in `5712` behavior for repeated failures; your installed ruleset is authoritative. Do not assume the sample fires on every kind of failed login. Invalid users, valid users with incorrect passwords, and Windows logon failures are distinct event paths.

This is for learning, not a production detection. Test positive and negative examples, verify threshold and time window, and review false positives before relying on it.

## Suggested portfolio description
> Built a local Wazuh SIEM home lab to collect and review authentication events, validate rule behavior with Wazuh Logtest, and document a repeat-failure investigation. (Describe only what you personally completed; add platform, version, and endpoint details only after you verify them.)

## Official references
- [Wazuh Quickstart](https://documentation.wazuh.com/current/quickstart.html)
- [Wazuh installation guide](https://documentation.wazuh.com/current/installation-guide/index.html)
- [Wazuh custom rules](https://documentation.wazuh.com/current/user-manual/ruleset/rules/custom.html)
- [Testing decoders and rules](https://documentation.wazuh.com/current/user-manual/ruleset/testing.html)
- [wazuh-logtest reference](https://documentation.wazuh.com/current/user-manual/reference/tools/wazuh-logtest.html)

Documentation URLs and requirements can change. Check the version selector and docs matching your installed release before following commands.

## Project status
Follow the setup guide to build your lab, validate the rules in your Wazuh version, and document your own results.
