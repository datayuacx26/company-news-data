---
schema_version: "1.0.0"
document_id: "7247903df0d4ff12e977070e2d7ea7e1fe587cd25d4eb64e659c37e2373a3b45"
company_key: "yc-canvas"
company: "Canvas"
source_id: "yc-canvas-news-import-8207a64dd502"
canonical_url: "https://www.canvas.inc/research/privileged-self-distillation"
published_at: null
first_seen_at: "2026-08-09T20:23:24.393484+00:00"
fetched_at: "2026-08-09T20:23:25.334371+00:00"
content_hash: "sha256:ebc1c0c213dc179dfdc22c13c36d6ae9e1b32eebb614d2fca5e05d986165449b"
---

# Privileged On-Policy Self-Distillation

## Introduction


Large language models are increasingly used for agentic tasks such as coding, research, and knowledge work, where success depends on carrying out a sequence of decisions over time. Post-training has driven much of this shift by teaching models to follow instructions, solve multi-step problems, and use tools effectively.


Post-training methods can be categorized across two axes:


- Where do the training states come from? on-policy vs off-policy


- How is that feedback used as supervision? outcome signal vs constructed signal


**Off-policy** methods train on data generated outside the student policy. A common example is supervised fine-tuning (SFT) on teacher trajectories or fixed datasets. These sources provide clean token-level targets, but they have a coverage limitation: states selected by the external source may not include the mistakes the student encounters in its own rollouts.


**On-policy** methods address this coverage gap by drawing training states from the student's own rollouts. In RLVR, a verifier scores each attempt. One common algorithm is group relative policy optimization (GRPO), which compares rewards within each sampled group.


Consider a simple scheduling task with two sampled rollouts.


prompt


Schedule a meeting with Maya tomorrow at 8 PM.


Rollout A


1


open calendar


2


set 8 AM


3


confirm 8 AM


reward: 0


Rollout B


1


open calendar


2


set 8 PM


3


confirm 8 PM


reward: 1


The verifier marks A as failed and B as successful, so GRPO favors rollouts like B over rollouts like A. This is useful because the policy learns directly from the outcomes of its own attempts. But the signal is coarse: each rollout receives only one binary outcome for the entire trajectory, telling us which attempt worked but not that the AM/PM choice mattered. A second limitation appears when both rollouts set 8 AM: their identical rewards leave the group with zero reward variance and no learning signal.


[On-policy distillation (OPD)](https://thinkingmachines.ai/blog/on-policy-distillation/) provides a denser alternative by replacing the single outcome with local teacher targets. A stronger teacher is queried at the same prefixes the student visited, providing dense token-level supervision without requiring reward variation. In the scheduling example, the teacher scores each decision in the failed rollout.


student


1


open calendar


2


set 8 AM


3


confirm 8 AM


↓


↓


↓


teacher


agrees


open calendar


prefers 8 PM


strong correction


locally agrees


given existing 8 AM history


The dense targets reveal where the teacher disagrees: opening the calendar receives little correction, while setting 8 AM receives the main correction as the teacher shifts probability toward 8 PM. OPD has a structural limitation: after a wrong choice enters the trajectory, later teacher queries remain conditioned on the history it creates. Confirming 8 AM is wrong for the task but locally coherent once 8 AM is already in the history, so it receives almost no correction.


This is **prefix drift** : after an early mistake, later teacher queries are conditioned on the history that mistake created. Those targets can still teach useful recovery behavior, but they describe continuations of the student's branch rather than a corrected trajectory.


Cursor's[Composer 2.5 writeup](https://cursor.com/blog/composer-2-5) addresses this by using hinted distillation to give the teacher extra context at the mistaken decision. Place a short hint before the target message, query the teacher under that privileged context, and train the unhinted student toward the resulting distribution. Here, the hint is a reminder to check AM/PM, while the student remains unhinted.


Hinted teacher


1


open calendar


check AM/PM


2


set 8 PM


3


confirm 8 PM


KL


↓


Student


1


open calendar


2


set 8 AM


3


confirm 8 AM


Hinted distillation shows how to transfer a correction, but it does not tell us what hint to use or where to place it. Think of a teacher correcting a student's mistake: the goal is to reveal the misconception without solving the problem for them. An overly specific hint may fix the problem in front of the student without teaching a reusable strategy. A vague hint may not explain what went wrong. The useful hint lies between these extremes and must arrive while the trajectory can still recover.


A failed on-policy trajectory offers the raw material for repair: the student's actions, the environment's responses, and the prefixes where recovery may still be possible. The challenge is to construct an intervention that fixes the trajectory and yields a target the unhinted policy can learn. We address this challenge with PSD.
