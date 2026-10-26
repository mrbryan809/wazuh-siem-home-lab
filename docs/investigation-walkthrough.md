# Investigation walkthrough: repeated SSH failures

This uses a synthetic log record and Wazuh rule tester. It does not represent a real incident or deployed lab.

## Scenario
The dashboard shows repeated SSH invalid-user events for a Linux endpoint. Establish what the alert says, what it does not prove, and what follow-up evidence would help.

## 1. Confirm the event
Use the sample line in `../samples/ssh-failed-login.log` in Ruleset test or `wazuh-logtest`. Inspect:
- **Raw event:** preserve the original line for notes.
- **Decoder:** verify it is parsed as SSH/syslog in your version.
- **Rule:** record the matched ID and description. Current official docs show `5710` for an invalid-user SSH line; verify locally.
- **Fields:** note endpoint/hostname, username, source IP, timestamp, and source port if decoded.
- **Correlation:** test repeated lines in the same Logtest session and determine whether the custom rule matches. Record the count and timeframe. Example threshold is five parent events within 120 seconds; validate actual behavior in your release.

A Logtest match validates parsing/rules against supplied text. It does not prove that a real endpoint generated or delivered the event.

## 2. Scope the activity
For a real lab alert, check a narrow time range around the event. Search the endpoint and source IP for related failures and successes. Check whether the username exists, whether other endpoints show similar activity, and whether the source is expected lab activity. Do not conclude intent from an IP address alone; NAT and shared networks can mislead.

Treat external reputation/geolocation enrichment as a clue, not proof. Record the source and timestamp if used.

## 3. Assess impact and next steps
Ask whether a successful login followed the failures, whether the account was valid, and whether there are independent signs of access. For a learning lab, document a benign conclusion when the only evidence is synthetic. In a real environment, follow organizational escalation and evidence-preservation procedures; do not disable accounts or block addresses unless authorized.

## 4. Write the result
Copy `../templates/incident-report.md`. Label evidence as synthetic, test-generated, or observed. Include Wazuh version, exact rule ID, query window, and limitations. Remove private addresses, usernames, tokens, and host details before publishing.
