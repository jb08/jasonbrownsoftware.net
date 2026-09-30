---
title: 'Async Workflows: Sidekiq vs. Temporal'
description: 'An election-themed comparison of three ways to run multi-step background processes in Ruby, from my Rocky Mountain Ruby 2026 lightning talk'
pubDate: 2026-09-30
---

<div style="background: #fef3c7; border: 1px solid #f59e0b; border-radius: 8px; padding: 0.85em 1em; margin: 0 0 1.5em 0;">
This is a write-up of my Rocky Mountain Ruby 2026 lightning talk.
</div>

## The candidates

The November election is a month away. Here are the "candidates":

![A Denver ballot drop-off box](../../assets/rmr/election_box.png)

| Candidate     | Async job library                 | License                   |
| ------------- | --------------------------------- | ------------------------- |
| Incumbent     | Sidekiq                           | MIT                       |
| Challenger    | Temporal                          | MIT                       |
| Up and comers | Postgres-backed durable execution | MIT / Apache 2.0 / varies |

Sidekiq is the Ruby standard for background jobs since 2012.

Temporal is the up-and-comer, with a Ruby-sdk as of September 2025.

The up and comers are a growing group: DBOS, pg_durable by Microsoft, ResonateHQ, Absurd Workflows, Obelis, and others. They promise durable execution managed via a relational database rather than a separate orchestration server.

## The demo

The demo is of running a _background check process_, which is comprised of 10 async jobs that must complete for a Job candidate to be accepted, start work, and get their first paycheck.

The demo ran in Sidekiq, Temporal, and Postgres-backed durable execution (DBOS).

## The incumbent: Sidekiq

![Sidekiq Web UI showing 30 enqueued BackgroundCheck jobs in the default queue](../../assets/rmr/sidekiq_index.png)

Tracing one background check across many jobs is difficult. We call it the "daisy chain", where one `Background Job` triggers another `Background Job`:

![A tangle of daisy-chained power strips and cables](../../assets/rmr/sidekiq_daisy_chain.png)

## The challenger: Temporal

A Workflow is one thread that ties all the `Activities` together: 

![Temporal timeline of a background check workflow, with parallel SendSearchActivity runs](../../assets/rmr/temporal_workflow.png)

### Durability: configurable retries

Here the `SendSearchActivity` is failing:

![Temporal timeline showing SendSearchActivity at attempt 6 while other Activities have completed](../../assets/rmr/failing_activities.png)

After the bug is fixed, the `Activity` and `Workflow` complete successfully:

![Temporal timeline showing SendSearchActivity succeeding on the 7th attempt and the remaining Activities completing](../../assets/rmr/retried_activities_succeeded.png)

### Managing agent workflows

Temporal is also a good fit for agent workflows: failing API requests, and remembering the AI conversation context, with ability to resume days or weeks later.

![Temporal event history of an agent workflow that creates a Zendesk ticket and prompts an AI](../../assets/rmr/temporal_good_for_agent_workflows.png)

### Cross-language support

The last `Activity` is handled by a Rust worker:

![Temporal Activity Task Scheduled event for HandlePossibleFcraDisputeActivity on the background-check-rust task queue](../../assets/rmr/cross_language_support_rust.png)

### Traces to Honeycomb

![Honeycomb traces for the StartWorkflow and RunActivity spans of the background check workflow](../../assets/rmr/honeycomb_temporal_traces.png)

## The up and comers: DBOS

The same background check running as a Postgres-backed DBOS workflow: 

![Terminal output of a DBOS background check workflow for Harry Kane](../../assets/rmr/dbos.png)

## Election Results

The informal "election" poll at RMR was close. With a plurality although not a majority, the winnner was.... Sidekiq! 🗳️