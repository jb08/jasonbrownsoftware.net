---
title: 'Notes on AI in Software Development (Feb 2026)'
description: 'AI prediction of trending up / trending down'
pubDate: 2026-02-02
---

With AI changing software development so rapidly, I thought I’d jot down my thoughts on it as of February 2026. Of course, with the pace of change, many of these ideas will be outdated, or so obvious they weren’t worth writing, in the not-too-distant future.

I’m going to group the ideas into predictions of <span style="color: green;">trending up</span> / <span style="color: red;">trending down</span>.

## <span style="color: green;">Trending Up</span>

### LLM performance

First off, LLM Agents perform so much more accurately than even 3 months ago. Back then, for instance, if you asked it to write a Rails database migration, it would prepend `202302…` because that was the date it trained on. Today, it's much smarter at picking today's date. Similarly, I recall loops of mis-code where it would continually get the wrong answer: “No, the table column still is not visible when scrolling right!” Today, it gets to the right answer with fewer prompts.

### Stories written for Developers’ Agents

Markdown is the lingua-franca of AI, and I’ve started grooming stories in Markdown format, attaching ample code snippets and links. This helps not only the Developer gain context on the story, but is easily consumable by the Developer’s Agent, too. In the past I may have been lighter on the details, knowing it could be communicated in grooming meetings or verbally, or from context they already knew. Now, I’m writing stories more prescriptively with more context, to speed up development for the Developer _and_ the Agent.

### API-Documentation for Agents

Similarly, I think most API documentation will be re-written or at least easily downloadable for consumption by an Agent. Many current API docs are written with the three-panel layout (left panel navigation, central panel documentation, right panel examples), which is a particularly difficult format to paste into an Agent console. If I were a developer building a new API integration today, the first thing I would do is paste the API documentation into a markdown file to paste into Cursor. 

### Git cloning Open Source libraries

When working in third-party libraries, I like cloning their open-source library, adding it to my Cursor workspace, and then asking questions with my code and the open-source library. So, I’m more frequently pulling the Rails, Temporal, and other third-party libraries locally. They’re great context for Agent prompts.

### 5-10x developers

How much more productive are we than we were pre-AI? I would estimate, 10x more productive on an individual task (and maybe 20x on an unfamiliar one, for me for instance integrating a css library). When grouped together with all other Developer activities (support, questions, maintenance, grooming, pairing, etc.), I’d estimate that translates to a net 5x increase in productivity. We’re all 5-10x developers now! While the glass-half-empty perspective might interpret the increase in productivity to reduce the need for developers, I would argue that this more directly translates into more and better software for companies and customers. Rather than reduce the value of software engineers, AI allows us to serve as “multipliers.”

### Command-line interfaces (CLIs)

CLIs historically were incredibly powerful but difficult to onboard and find all the relevant features. Using CLIs with AI and with the CLI open source code amounts to really powerful and efficient tooling.

## <span style="color: red;"> Over-hyped / Trending Down</span>

### Multi-Agent

Codex, OpenCode, Cursor’s updates, all point towards a human working simultaneously with multiple Agents. I just don’t trust them enough, or can’t switch contexts quickly enough, to justify that. My workflow is still single-threaded. It seems like answers are coming back in 10-60 seconds, and I, the human, am still the bottleneck in the process. That said, I am also the one responsible for the code accuracy and quality.

### MCPs

Six months ago, you would have thought we’d all be using many MCPs, but it hasn’t panned out. I’ve seen Gitlab MCP used for efficiently opening MRs, but haven’t personally seen other MCP usage. CLIs are often faster and more accurate. Maybe the new _skills.md_ will be the next and better iteration.

### AI-less experiences

Experiencing the productivity gains of AI in software development, I’m now skeptical of AI-less experiences. It’s like going back in time! Datadog, for instance, still remains a tool of “I’ve seen a coworker do this one thing, therefore I know how to do it”. Similarly, JIRA Support management still primarily remains a “I’ve seen a support ticket like this before, or I know where to look for a runbook.” While we’ve started to build tools around it, JIRA itself could be so much more helpful with “here’s three related support tickets, linked runbooks, and here’s likely the fix you’ll need.”

### Wispr Flow

Will Developers dictate code in the future? Certainly, if you have AI filling the syntax and details, it does seem fast(er). However, it would be very noisy in a hybrid/in-office environment. Dictating notes has certainly taken hold in the medical industry, but for my voice’s sake, I think I’m hoping this one doesn’t pan out.

### AI writing for human consumption

AI writing puts the burden on the reader. Oftentimes I’ll pick up a JIRA story that says to do X,Y,Z like run a benchmark on code speed, and I’ll talk with the story creator and they’ll say, “oh the AI added that, that isn’t necessary.” When writing something that you know another human will read, I prefer the human writing, and if needed, the AI editing. But AI-first generated writing can be overly long, dramatic, and wasteful of the readers’ time and attention. But I’m all for AI coding for machine-consumption.

### AI summarizing meeting notes

Zoom records and transcribes all my meetings, and you know how many times I’ve re-read them? Zero times. It is possible as an IC I am not in meetings that frequently, and my experience may differ from manager’s experiences. I guess I could see piping Granola-like AI notes into story grooming or elsewhere, but even then I think the notes would be too verbose. I believe human-written updates and action items to be more accurate.

-- 

With the pace of change, I wish I had written this a few days ago so that I could say “Jan 2026”, and be right, or less-wrong, or less-obvious, for slightly longer. As it stands most of these takes will be different or out-of-date by end-of-month!


