**English** · [Français](README.fr.md)

> Showcase repository: the code and data of this project are private; this README describes it.

# Agent fleet management

A coordination post that tracks, across several machines, the coding-agent sessions at work, what they are waiting for and the state of the machines that run them.

## The problem

Anthony runs several agent sessions in parallel on a server, a laptop and shared compute workstations. Without tracking, he had to chase each agent, restart the ones that had stopped and receive results announced too early or wrong. Fleet management centralises this supervision: a dashboard tells him who is working, who is waiting for him, and why.

## How it works

- The dashboard is regenerated every 5 minutes: sessions and their state (working, waiting, paused), status of each project, compute workstations and their reservations, quota, shared decisions, coordination log.
- A summary of each session is written every 30 minutes.
- A request from Anthony goes through a router: it points to the open session concerned, or opens one with a model and an effort level chosen by difficulty (simple, medium, hard).
- A decision that applies to every session is published once, then injected into each session, on each machine.
- Before a result reaches Anthony, a fresh read-only agent checks it; if it disputes the result, the session is restarted for one correction at most.
- Every Monday at 8 am, a weekly report measures session autonomy, restarts, retractions, disputed results and hand-backs to Anthony.

## What is built

In service since 29 October 2026. Functions in place:

- Dashboard, summaries, routing, shared decisions, verification before reporting and weekly report.
- Automatic closing, after one hour, of sessions whose remote-control link is cut (checked every 10 minutes).
- Network watchdog on 3 compute workstations: it checks the link every 5 minutes and repairs it on its own. During test outages, it restored each workstation in 3 min 26 s to 4 min 24 s.

Report of 2 November 2026: overall autonomy of 47%, 114 restarts, 18 retractions, 64 results disputed by the verifier, 17 hand-backs to Anthony.

Known limit: the evaluation only reads conversations held on the server.

## What is public

This page is public. The dashboard, its code and its data stay private: they describe an institution's machines.

## Tech stack

- Python (dashboard, routing, summaries, report)
- TypeScript (agent tool extensions: shared decisions, verification)
- cron, tmux
- Tailscale
- Windows batch scripts and scheduled tasks (network watchdog)
- HTML and CSS
