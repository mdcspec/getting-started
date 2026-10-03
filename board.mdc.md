---
mdc: "0.1"
title: Ship the signup flow
---

# Ship the signup flow

A normal GitHub checklist — and a task graph the `mdc` CLI (and your agents) can query and edit. Open it on GitHub to see it render as real checkboxes.

## Design
- [x] Agree on the flow {#design @sam done=2026-09-10}

## Build
- [ ] Email + password form {#form needs=design}
- [ ] Verification email {#verify needs=design}
- [ ] Wire form → API {#wire needs=form,verify}

## Ship
- [ ] End-to-end test {#e2e needs=wire}
- [ ] Deploy to production {#deploy .gate needs=e2e}
