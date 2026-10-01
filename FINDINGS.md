# Findings & Analysis

## Summary

This document presents the key findings from running the Cowrie SSH honeypot. Data was collected over the deployment period and analyzed to identify attacker behavior patterns.

## Attack Statistics

> **TODO:** Add your specific numbers below from your honeypot logs.

| Metric                        | Value       |
|-------------------------------|-------------|
| Total login attempts          |             |
| Unique source IPs             |             |
| Unique usernames tried        |             |
| Unique passwords tried        |             |
| Successful (fake) logins      |             |
| Deployment duration           |             |

## Top Attempted Credentials

### Most Common Usernames

> Add your top usernames here.

| Rank | Username | Attempts |
|------|----------|----------|
| 1    |          |          |
| 2    |          |          |
| 3    |          |          |
| 4    |          |          |
| 5    |          |          |

### Most Common Passwords

> Add your top passwords here.

| Rank | Password | Attempts |
|------|----------|----------|
| 1    |          |          |
| 2    |          |          |
| 3    |          |          |
| 4    |          |          |
| 5    |          |          |

## Geographic Distribution

> Add geographic breakdown of attack source IPs.

## Post-Login Behavior

> Document commands attackers ran after gaining access to the fake shell.

Common commands observed:
-
-
-

## Attack Timing Patterns

> Note any patterns in when attacks occurred (time of day, day of week, spikes).

## Key Takeaways

1.
2.
3.

## Recommendations

Based on the findings from this honeypot deployment:

1. **Use strong, unique passwords** — the most common credentials attempted are simple defaults
2. **Disable root SSH login** — a significant portion of attempts target the root user
3. **Use key-based authentication** — eliminates brute-force password attacks entirely
4. **Implement fail2ban or similar** — rate-limit repeated failed login attempts
5. **Monitor logs actively** — automated attacks are constant and begin within minutes of deployment
