---
name: erasure
argument-hint: "[path or module — omit to scope to the current diff]"
description: Structural cleanup after a large change — remove concepts, rules, and code that shouldn't exist by re-deriving from purpose instead of editing in place. Use when the user says "erase", "/erasure", or wants accreted bloat, redundant abstractions, or patches-on-patches removed after a multi-file change. Not line-level polish.
---

# Erasure

Erasure is the removal of unnecessary concepts, rules, and structure so a system
becomes fundamentally simpler. It is deletion, not tidying: the output of this
skill is code that no longer exists, and abstractions that replaced several
things with one.

## Why this is a procedure, not an instruction

You have an accretion bias. Adding code is locally safe; removing it risks
breaking an invariant you can't see — so left to your defaults, you add, patch,
and keep. Worse, code you are looking at anchors you: any "rewrite" collapses
back toward the original text. You cannot be talked out of this bias, but you
can be routed around it: **you cannot keep what you do not see.** Every step
below exists to keep the original text out of view while you decide what
deserves to exist.

## Scope

- **No argument** (default): the current change — working tree plus commits
  since the merge-base with the main branch. This code has no history to
  respect; you or this branch wrote it, so erase aggressively.
- **A path or module argument**: full erasure of that target, including old
  code. Here Chesterton's fence applies: before removing something weird,
  check `git log`/blame for the reason it exists. Weirdness with a reason
  stays (and gains a comment); weirdness without one dies.

## Procedure

1. **Inventory.** Map the scope into regions: functions, types, config
   surfaces, modules. Note every concept the change introduced — each new
   type, parameter, flag, layer, and file is a suspect.

2. **Spec before sight.** For each region, write down what it is *for* — its
   contract: inputs, outputs, invariants, the one job it exists to do. Write
   the contract from the callers' needs, not by paraphrasing the
   implementation. Do this for all regions before editing anything.

3. **Re-derive.** With the original implementation deliberately off-screen,
   draft each region fresh from its contract — in a scratch file if that
   helps. Then diff your derivation against the original. Everything present
   in the original but absent from the derivation must justify itself or be
   deleted. The question is never "can this be improved?" — it is **"would
   this exist if written fresh from the spec?"**

4. **Hunt accretion signatures.** These almost never survive re-derivation;
   check for them explicitly:
   - parameters, branches, and config flags nothing exercises
   - fallbacks and guards for states that cannot occur
   - compatibility shims for code that no longer exists
   - two abstractions doing one job (unify or delete one)
   - a patch whose root-cause fix would erase the patch — prefer the fix
   - indirection with a single caller and no second one in sight

5. **Verify.** Erasure preserves behavior. Run the tests and typecheck; if
   neither exists for the touched code, say so and mark the result
   unverified rather than claiming safety.

## Measure

Complexity is the number of branches — `if`, `match`, `case`, ternaries,
boolean guards — and the number of concepts: types, parameters, flags, layers,
files. Reducing these requires better abstractions and cannot be faked.
Never optimize LOC directly: shortening names, stripping comments, or
compressing lines is minification, not erasure, and counts as failure.

## Report

State what no longer exists and why it shouldn't have, the branch/concept
delta, and verification status. If nothing deserves erasure, say exactly
that — a forced deletion is worse than a null result.

## Boundary

This skill removes structure; it does not polish lines. Naming, formatting,
idiom, and comment quality are a different kind of pass. If you find only
that kind of work in scope, report the null result and name it as polish
work rather than doing it here.
