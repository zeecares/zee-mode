---
name: create-verification-skill
description: "Generate a project-local verification skill that drives the real app the way a user does - any language, framework, or platform. Use when a project has no scripted way to prove UI, CLI, or service behavior."
---

# Create a verification skill

Every serious project needs a scripted way to drive the real app and prove
behavior: launch it, exercise a feature the way a user would, and capture
evidence. This skill generates that as a project-local skill tailored to the
repo. Write the generator's output for the next agent, not for a human: it
will be read cold, mid-task, by an agent that has never seen the app.

## 1. Interview the repo, not the user

Answer these from the codebase. Ask the user only what you cannot observe.

- **Surface:** what does a user actually touch? A web UI, a CLI, a desktop
  app, an API, a library? A repo can have several; pick the primary one and
  note the rest.
- **Run:** how does the app start locally? Prefer the repo's own documented
  dev command. Note ports, env vars, seed data, auth.
- **Drive:** how can an agent interact with it programmatically? Existing
  harnesses first - Playwright specs, expect scripts, PTY helpers, curl-able
  endpoints, a debug port. Only then pick a generic recipe: a browser driver
  for web, a tmux/PTY harness for CLI, plain HTTP for services.
- **Observe:** what evidence can be captured? Screenshots, terminal
  transcripts, response bodies, logs, exit codes, database state.
- **Isolate:** can two instances run side by side? If not, say so in the
  generated skill. Refusing to double-drive a shared instance beats
  corrupting a live session.

If the checkout does not build or start as-is, fix that first, or report it
precisely. A skill written against a broken base teaches wrong steps.

## 2. Generate the skill

Write `skills/verify-<app>/SKILL.md` inside the consuming repo - or the agent
host's skills directory (`.claude/skills/`, `.cursor/skills/`) when the repo
has no skills convention of its own. Use YAML frontmatter (`name:
verify-<app>` plus a description naming the app, the surface, and when to
reach for it; without frontmatter some hosts never register the skill) and
these sections, each grounded in what the interview actually found. No
placeholders left.

- **Launch:** the exact command that starts the app for verification, and how
  to tell it is ready - a log line, a port answering, a prompt. Include
  teardown. For a short-lived CLI there is no server: launch means build the
  binary once, then start each drive in its own isolated session.
- **Doctor:** one read-only check answering "is this instance worth
  driving?" - process up, right build, port owned by us, auth valid. Run this
  first whenever anything looks off.
- **Drive:** the harness recipe with real selectors and commands from this
  repo, not examples. Prefer stable handles (ARIA labels, data attributes,
  prompt strings, route paths) over coordinates and tab order.
- **Evidence:** what to capture for a proof and where it goes. Exercise the
  real user path, not internal setters or test-only endpoints. Capture the
  action and the resulting state, not just the final screen. Verify side
  effects - files written, rows inserted, messages sent - alongside what is
  visible. Mocks only where a production boundary already isolates the
  external system.
- **Cleanup:** how to tear down what the run created. Never kill by process
  name; kill what you started. Cleanup removes instances and scratch state,
  never the evidence: proof artifacts survive teardown, in a location the
  skill names.
- **Helpers:** any script the skill ships is executable and its invocation is
  shown in the skill body. A helper the reader has to reverse-engineer is not
  a helper.

## 3. Seed the feature map

Create `skills/verify-<app>/features/README.md` plus one file per user-facing
feature you can identify - aim for the top three to five, from routes,
commands, menus, or docs. Each file answers, from the user's point of view:
what the feature is, how to reach it, how to drive it with the harness, and
what observable end state proves it works. The map is the repo's maintained
verification source; a proof that drives one convenient entry point is
incomplete when the map lists others.

## 4. Prove the generated skill before handing it over

Run its own instructions end to end once: launch, doctor, drive ONE mapped
feature, capture evidence, clean up. After cleanup, confirm the evidence
still exists at the named location - a cleanup that eats the proof fails this
step. Fix what fails, and run cleanup after every failed iteration too, so
broken attempts do not strand processes and ports.

A generated skill that was never executed is a draft, not a deliverable.

## 5. Keep the map honest

As the app changes, the feature map drifts. When a proof fails because the
app moved, update the map in the same change. The map is source, not
documentation: treat a stale map as a failing test.
