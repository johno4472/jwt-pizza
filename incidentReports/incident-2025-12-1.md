# Incident: 2025-12-01 12-24-22

## Summary

> [!NOTE]
> Write a summary of the incident in a few sentences. Include what happened, why, the severity of the incident and how long the impact lasted.

```md
**EXAMPLE**:

Between the hour of 12:23 and 12:25 on 12/1/2025, 7 users encountered failed pizza orders. The event was triggered by what I assume is a triggering of the chaos monkey endpoint at 12:23. The triggering of chaos monkey contained no modifications to existing code, just the call of an existing endpoint to simulate pizza failures.

A bug in this code caused internal server erros caused upon pizza creation, because of a chaos monkey error thrown. The event was detected by grafana onCall, which notified me of an unusual amount of pizza failures. The I started working on the event by looking at the logs for any unique error messages, because I received status codes of 500 when trying to autonomously create pizzas. I then filtered to only see logs with 500 level errors and there were the 7 failed pizza orders, with a corresponding link to end the chaos. This mild/moderate severity incident affected 53.8% of users during that time, because 6 out of 13 order attempts during this time were successful. I would grade it as a higher severity incident, but the issue was resolved in a matter of minutes so not many users were affected.

There was no further impact as far as my resources can tell, and all things are back to normal.
```

## Detection

> [!NOTE]
> When did the team detect the incident? How did they know it was happening? How could we improve time-to-detection? Consider: How would we have cut that time by half?

```md
**EXAMPLE**:

This incident was detected when the PizzaFailure Alert was triggered and I (John Olson) was paged.

No one else was notified because I responded immediately and was able to fix the error.

I will modify the alerts that I have so that I do not ignore a valid alert because of too many invalid alerts. For example, every time I redeployed my service, I received several alerts warning me about temporarily high latency, which was an alert that meant nothing. I was also warned about lack of data surrounding endpoints and pizza latency. This was frustrating, because for some reason my configurations led to me being alerted for information I didn't care about. The one most useful alert was a high amount of pizza failures, which never gave me a false positive, so it was the most accurate alert to find the issue, though it is a lot easier to ignore an alert when useless alerts are firing.
```

## Impact

> [!NOTE]
> Describe how the incident impacted internal and external users during the incident. Include how many support cases were raised.

```md
**EXAMPLE**:

For 0 hrs 2 minutes between 19:23 UTC and 19:25 UTC on 12/01/25, about half of all pizza orders failed.

This incident affected 7 customers (53.4% OF USERS ENGAGING WITH THE ORDER PIZZA ENDPOINT), who experienced unexplained server errors leading to the failure of pizza creation.

0 SUPPORT TICKETS AND 0 NUMBER OF SOCIAL MEDIA POSTS were submitted.
```

## Timeline

> [!NOTE]
> Detail the incident timeline. We recommend using UTC to standardize for timezones.
> Include any notable lead-up events, any starts of activity, the first known impact, and escalations. Note any decisions or changed made, and when the incident ended, along with any post-impact events of note.

```md
**EXAMPLE**:

All times are UTC.

- _19:23_ - Chaos endpoint is triggered
- _19:24_ - I notice alert on a jump in pizza failures
- _19:24_ - I inspect logs and see a 500 level error message containing a link to end the chaos
- _19:25_ - I follow the chaos link and the webpage notifies me that chaos is over
- _19:25_ - I return to the pizza factory webpage and verify that chaos has ended
- _19:25_ - I return to my grafana dashboard and see that pizza latency has decreased to a stable level, and so have pizza failures. Incident is resolved.
- _20:03_ - I write this exact line in my incident report
```

## Response

> [!NOTE]
> Who responded to the incident? When did they respond, and what did they do? Note any delays or obstacles to responding.

```md
**EXAMPLE**:

After receiving a page at 19:23 UTC, John Olson came online at 19:23 UTC in Grafana to inspect the issue, and subsequently inspect the jwt pizza service router for ordering a pizza.

John had a background in the order router of the pizza service, so no second alert was sent, and John was able to resolve the issue.
```

## Root cause

> [!NOTE]
> Note the final root cause of the incident, the thing identified that needs to change in order to prevent this class of incident from happening again.

```md
**EXAMPLE**:

I believe that the triggering of the chaos monkey endpoint is what cause the error. By setting `enableChaos` to `true`, a random 50% of pizza orders failed.
```

## Resolution

> [!NOTE]
> Describe how the service was restored and the incident was deemed over. Detail how the service was successfully restored and you knew how what steps you needed to take to recovery.
> Depending on the scenario, consider these questions: How could you improve time to mitigation? How could you have cut that time by half?

```md
**EXAMPLE**:
By following the link in the logs attached to the failed pizza order requests (with status codes of 500), John was able to quickly stop the chaos and resolve all issues.

I was abel to resolve this issue rather quickly, because I was looking and waiting for an error to happen, and had an idea of where I would find the solution, because I had found in the code that there was a variable created to store the link to follow to disable the chaos.

If I didn't know this, however, and wasn't waiting for the issue to arise, I could speed up mitigation time by attaching an alert to a high number of internal server errors, because those have pretty much been unique to this kind of issue.
```

## Prevention

> [!NOTE]
> Now that you know the root cause, can you look back and see any other incidents that could have the same root cause? If yes, note what mitigation was attempted in those incidents and ask why this incident occurred again.

```md
**EXAMPLE**:

This root cause has only led to other issues in testing and intentionally injecting chaos myself, but no real errors in production like this one.
```

## Action items

> [!NOTE]
> Describe the corrective action ordered to prevent this class of incident in the future. Note who is responsible and when they have to complete the work and where that work is being tracked.

```md
**EXAMPLE**:

1. Create an alert for an abnormal amount of internal server errors
1. Scan for other security vulnerabilities in my routers (like removing the chaos monkey if it were a real issue)
1. Figure out how to have normal traffic stay within the expected realm (a grafana dashboard and metrics config issue) so I don't have too many useless alarms so I pay more attention to the real ones.
```
