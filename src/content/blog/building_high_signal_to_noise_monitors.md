---
title: 'Building high signal-to-noise Monitors'
description: ''
pubDate: 2026-08-31
---

Team A receives 50 Pages per week, or an average of 7 per day. Maybe five are during the workday, one after work, and one overnight. They have zero-to-one real incidents per week, of varying severity, for a signal-to-noise (“SNR”) of 1:50. Their monitoring noise is 50 times stronger than the signal (and in the case where they have zero real incidents, the noise is infinitely stronger than the signal).

Team B receives 4 Pages per week, zero-to-one of which is deemed a real incident of varying priority. Their monitoring SNR is 1:3. Their monitoring noise is 3 times stronger than the signal.

***What are the goals of a monitoring system? Which team has a better monitoring system? And why are the vast majority of teams like team A?***

## Goals of a monitoring system

Overall a reliability system should have the following primary goals:

1. Determine if the system is experiencing problems before your customers do (otherwise they won’t remain customers). This is called mean-time-to-detection (“MTTD”).
2. Quickly resolve those issues. This is called mean-time-to-resolution (“MTTR”).
3. Don’t have many outages. Reliability is the #1 feature that Customers want. This is called mean-time-between-failures (“MTBF”).
4. System health should be timely and accurately updated on a status page (“#transparency”).

### Secondary goals

Secondary goals are more debatable. This post will argue that these secondary goals facilitate the primary goals:

- **Monitors should be Actionable.** They should require specific, timely investigation and action. Monitors ping all hours of the day and all days of the week; working hours, at the grocery store, at the gym, dinnertime, and overnight. They need to be Actionable, otherwise we are teaching Engineers to disregard them. Maybe a criteria for a monitor should be, “Would you get out of bed for this?”
- **Monitors should have a high signal-to-noise ratio.** An SNR measurement compares the level of a useful signal to the level of unwanted background noise. An SNR of 1:9 means that the unwanted background noise is 9 times stronger than the useful signal. An SNR of 4:0 means there is absolutely zero background noise, resulting in a theoretically perfect signal. The more background noise, the more akin monitors are to “The boy who cried wolf”.

## The Problem

At any given moment, most software companies have _hundreds_ of monitors in an actively firing state, including dozens in the P1-P3 categories. But not an ongoing incident. Why?



### Monitor Ownership

Most teams have 100-200 monitors. Half of those monitors were created by engineers no longer working on the team. When an on-call engineer receives a Page, they generally won’t feel empowered to change that monitor. They could start a slack thread about changing it, but they have many competing priorities, and they’re getting many pages per day.

Many monitor alerts might as well be written in French. Teams often have a wide area of coverage, maybe 7-10 services, and most engineers have only worked on 1-3 of those services (some of the services may not have been actively developed recently). _High latency on Service Z? This is the seventh alert I’ve received today, and I don’t even know what Service Z does._

Monitors could benefit from clear ownership; possibly, the monitor owner should be notified via email that their monitor alerted. If they receive this email many times, they will know best how to change the monitor – **its duration, threshold, priority, downtime,** or otherwise. If team A’s monitors emailed their monitor owner, and had clear per-person monitor ownership, through tuning and pruning, would their team become more like team B?

### On-call engineer priorities

On-call engineers priorities, _from engineering managers’ perspective_, are approximately the following:

1. Remediation of active incidents
2. Responding in a timely manner to all Pages
3. Not missing any incidents; though this is vaguer
4. Responsiveness in slack
5. Working through the support board, esp. high priority tickets
6. …
7. Tuning of monitors

On-call engineers’ priorities, _from their teammates’ perspective_, are the following: mostly the same list, except “Working through the support board” gets moved far higher, maybe to the top, as that is work that is being passed onto the rest of the team.

On-call engineers’ priorities, _from their perspective_, is the following: mostly the same list as the lists above, except also add in “survive the week” and “try to add in some sprint work to meet my quarterly project commitments.”

***Tuning of monitors is near the bottom of all priorities. An on-call engineer only has to deal with the noise once every six weeks.***

### Who will drive improvements?

_Engineering managers_ are graded based on their team’s ability to deliver projects, how they develop their team, how they collaborate, and how their system’s reliability, among their many, many other responsibilities.

Meanwhile, _newer engineers_ regardless of level may not have the system context or ownership clarity to tune or even remove monitors. They may receive 50 Pages a week and assume “that’s just the way it is and has to be”.

Improvements in teams’ reliability systems, if they are to occur, must then be led by team’s _longer-tenured engineers_. These engineers feel more confident driving these changes.

### Proposals

If we agree that higher SNR monitors are a worthy goal, here are some proposals for improvement:

- **Ownership:** All monitors have a person “owner”, and that owner is emailed whenever the monitor alerts.
- **Auto-escalation:** Monitor alerts are treated seriously by the company; this has happened at Checkr and it has improved the tuning of monitors (in fact I should note that much of my thinking in this post is from what I've learned from some very senior Checkr engineers and teams). P1-P3 monitors now open Incidents, ie. assume the alert is signal.
- **Auto-quarantining:** Shopify in a blog post wrote about how their CI suite would auto-quarantine flaky tests. Flaky tests and noisy monitors are similar: they could be a regression or an incident, respectively, _but probably aren’t_. If a noisy monitor is degrading the overall company’s alertness, should that monitor be auto-quarantined until re-tuned?
- **Root cause:** Fixing the root cause of a monitor alert is nice in theory but hard in practice. Most monitor root cause fixing would require either major system changes or if your system has high volume, hunting of endless edge-cases.
- **Reporting:** Teams are motivated by comparison; measurement of the SNR problem could drive change. But unfortunately it’s not a one-time thing.
- **Tooling improvements:** An APM system, such as Datadog, doesn’t know whether its monitor alerting turned into a real incident or real Action; those occur downstream and Datadog doesn’t have this _signal_ information. Also, it would be nice to be able to tune monitors via phone.
- **Criteria:** “Would you get out of bed for this?” System-wide issue: Yes!! Three Workflows out of a million are erroring? No. These need separate processes.
- **Clarity:** How do we define monitor “actionability”. All monitors should be Actionable. What exactly is monitor actionability?
- **Investment:** Some third-party companies are going to address this, similar to the many startups selling improvements to companies’ DORA metrics.

---

This marks the end of the blog-post. Hope this identifies a possible problem and possible solutions. Let me know your viewpoints, especially counter-points!

---

## Addendum

### How this relates to AI

With AI increasing software development productivity, the other areas of the software development lifecycle (“SDLC”) become even more important: planning, design, testing, deployment, maintenance, and reliability monitoring.

The company I work for, Checkr, is consistently improving many areas of its SDLC; Checkr’s testing and deployment CI/CD, which I previously wrote about <a href="https://medium.com/checkr/its-always-greener-on-the-other-pipeline-5139ed849aef" target="_blank" rel="noopener noreferrer">here</a>, is healthier than it's ever been; flaky tests are rare, developers rarely are required to manually “retry” CI jobs, and the CD duration from merge to production deploy is consistent and fast.

The rest of the AI SDLC, including reliability monitoring, is receiving more attention, too.

### Background and tooling

Many companies use <a href="https://www.datadoghq.com/" target="_blank" rel="noopener noreferrer">Datadog</a> for application performance monitoring systems (“APM”). Other APM providers include New Relic and Honeycomb.io. Datadog is where teams can build their monitors; most monitors measure metrics, and “alert” when a metric calculated over a duration exceeds a set threshold. Monitors have priorities including P1 (critical), P2 (high), P3 (medium), P4 (low), and P5 (info). Monitors are assigned to teams. Monitors can have scheduled downtime. Composite monitors are a type of monitor that alerts only if a set of other monitors is alerting. Monitors should link to an APM or custom dashboard to facilitate investigation.

Monitor examples include:

- Error rate on X endpoint exceeds 5% over 15 minutes
- No Completed Workflows in 2 hours
- Stuck Workflows found!
- Average latency exceeds 60 seconds over 5 minutes
- Low volume of requests on service B
- A recurring job failed!
- Average Workflow turnaround time (TAT) exceeds 10 minutes

In addition to Datadog for APM, companies can also use <a href="https://www.montecarlodata.com/" target="_blank" rel="noopener noreferrer">Monte Carlo</a> for database-based monitoring. It can group based on sets of criteria and alert if the sum or mix of a certain field is anomalous. Its Monitor examples include:

- Field X rate, grouped by category Y, is anomalous

<a href="https://incident.io/" target="_blank" rel="noopener noreferrer">Incident.io</a> or <a href="https://www.pagerduty.com/" target="_blank" rel="noopener noreferrer">Pagerduty</a> can be used for on-call management, incident response, and then postmortem process. This is where managers make on-call rotation schedules, and set rules for how alerting monitors are converted to Pages and routed to on-call engineers. Pages can be high-priority which make a loud noise on an on-call engineer’s phone, and require acknowledgement, otherwise they cycle to other engineers until acknowledged. Low priority pages generally do not make a loud noise, are often silenced overnight, and route to email or an alerts slack channel.

<a href="https://slack.com/" target="_blank" rel="noopener noreferrer">Slack</a>, <a href="https://www.microsoft.com/en-us/microsoft-teams/group-chat-software" target="_blank" rel="noopener noreferrer">Microsoft Teams</a>, and <a href="https://www.zoom.com/" target="_blank" rel="noopener noreferrer">Zoom</a> can be used for incident response coordination. High priority pages open triage incident Slack channels which can then be “accepted” into a real incident, which brings in stakeholders and starts a zoom call, or the triage incident can be declined. Triage incidents not accepted or declined over a set time period can be auto-converted into a real incident. Incidents are graded based on severity of impact, ranging from Sev-0 (System Outage), Sev-1 (Critical), Sev-2 (Major), and Sev-3 (Minor).
