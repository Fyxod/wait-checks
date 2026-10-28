---
name: wait-checks
description: Keep an active turn open while actually waiting, checking real progress, reporting, and repeating at the user's requested interval. Use for "wait checks", "continue wait checks", or requests to stay with ongoing work and check it after a real wait, in any project or chat. Distinguish this from a request for a future reminder or background-only scheduling.
---

# Wait Checks

Run the wait-and-check cycle in the current conversation. A running background
monitor, a promised later update, or a final reply saying monitoring continues
does not fulfill an active wait-check request.

## Establish the current contract

- Recover the latest requested interval, work being watched, reporting preference,
  and stopping condition from the conversation and available project memory.
  Use the user's units accurately. Do not hard-code a default interval.
- When the user says "continue", retain the existing interval and next deadline.
  Reconnect to the existing waiter or monitor when possible. Do not restart the
  timer, duplicate workers, or launch the work again just to resume checking it.
- If the interval or scope is genuinely missing, ask for that information. Do
  useful independent work while awaiting an answer; do not invent an interval.
- Watch the full scope the user requested. Completion of one job or delivery
  does not end a request that also covers remaining queued work or reviews.
  Do not expand that scope to unrelated work or all activity on a machine.
- Waiting grants no new permission to spend resources, publish, send messages,
  restart jobs, or change an experiment. Preserve the task's existing boundaries.

## Actually wait with the available tools

1. Read the current clock or a trustworthy existing timer. Establish the next
   deadline and a live wait handle. Use an attached terminal session when one is
   available; otherwise use an actual sleep/wait tool that keeps the turn active.
2. Let real time elapse. Keep the turn open and resume the same live handle through
   bounded tool waits so new user input can be handled promptly. Short tool yields
   are transport chunks of one wait, not permission to run extra progress checks.
3. Do not repeatedly query iterations, logs, dashboards, or job state between
   scheduled checks. An event waiter may observe explicit completion/error events
   and wake early for a meaningful event or necessary recovery. Never replace the
   requested interval with a busy loop of status queries.
4. At the scheduled wake, inspect the actual current state using the relevant
   available sources: a job receipt, process, test result, queue, artifact, external
   service, or the chat's agreed checkpoint. A timer firing alone proves only that
   the wait elapsed. Distinguish worker completion from any required output checks,
   review, delivery, or backup.
5. Give a concise, evidence-based update: completed work, remaining work, real
   errors or blockers, and the next check when useful. Follow the user's requested
   report format; when asked only for an ETA in minutes, answer in minutes.
6. Handle necessary actions within the existing authorization, then resume the
   wait. With an existing periodic monitor, retain its cadence. For a newly created
   loop without another timing rule, wait the requested interval after each check.
   Report a late or missed check honestly instead of inventing one or flooding the
   user with fictional catch-up checks.

Use a monotonic clock for elapsed time when available and wall-clock timestamps
for readable deadlines. Shorten only the remaining wait after an interruption;
do not count elapsed downtime as a check that the agent actually performed.

If the environment cannot perform a live wait or retain the turn, state that
limitation clearly. Offer an available alternative without claiming that a
reminder, detached process, or a later user message is the requested active wait.

## Handle steering and interruptions

- New questions or status requests steer the current task. Answer briefly, then
  continue the authorized wait cycle unless the user stops it or changes scope.
  An unrelated question does not silently reset the timer or cancel the work.
- Apply a changed interval or scope without resetting running work. Keep a user
  change to resource availability separate from the check cadence.
- After reconnecting or recovering context, inspect existing handles, receipts,
  and artifacts first. Replace a dead waiter only when needed; do not duplicate
  scientific, build, deployment, or other work being monitored.
- Where durable state is available and appropriate, record only what continuation
  needs: interval, watched scope, next deadline, live handle/PID, last actually
  inspected checkpoint, pending action, and relevant permissions. Keep credentials
  and unrelated chat or project data out of that record. A chat-only environment
  does not need a filesystem, Git repository, or a separate memory tool.

## Stop truthfully

End the cycle when the requested scope is complete, the user stops or replaces
it, or a genuine blocker requires user input and no useful authorized progress
remains. Handle any due deliverables or handoff before ending the turn.

Do not send a final response merely because background workers are running,
one milestone completed, a monitor was installed, or the wait feels long. If a
platform interruption ends the session, preserve available continuation state
and describe the interruption honestly; do not claim unattended checks occurred.

The user's explicit instructions take precedence over this skill's defaults.
