# TC-FEAT-001 — Checkpoint Respawn Preserves Mission State Correctly

**Feature:** Checkpoint respawn
**Area:** Gameplay QA, mission flow, state validation
**Priority:** High

## Why I would test this

Respawn looks like a small feature, but it usually touches several systems at once: player transform, physics state, mission timer, AI position, checkpoint ownership, lap/objective state, camera, audio and UI.

A respawn can be technically functional and still feel wrong if the player comes back facing the wrong direction, loses progress, gains unfair advantage, or returns in a confusing state.

## Test Objective

Verify that checkpoint respawn restores the player to a valid, readable and fair gameplay state without corrupting mission progression.

## Preconditions

* Mission/race contains multiple checkpoints.
* Player can fail, crash, fall out of bounds, or manually reset.
* Timer, objective state, AI opponents or mission scripting are active.
* Save/load, pause/resume or restart-from-checkpoint is available if supported.

## Test Steps

1. Start the mission from a clean state.
2. Reach checkpoint 1 and trigger a respawn immediately after crossing it.
3. Check player position, rotation, speed, camera orientation and input recovery.
4. Repeat the same check after later checkpoints.
5. Trigger respawn while:

   * airborne;
   * upside down;
   * off-track;
   * near AI opponents;
   * inside a collision-heavy area;
   * during a scripted event;
   * while a boost, debuff or temporary modifier is active.
6. Verify mission timer, objective state, lap count and UI after each respawn.
7. Restart from checkpoint, if supported.
8. Repeat after pause/resume or save/load.

## Expected Result

Respawn should place the player in a valid and understandable state.

The player should not respawn:

* facing the wrong direction;
* inside traffic or enemy AI;
* below the ground;
* outside the intended playable route;
* with invalid speed;
* with broken camera orientation;
* with reverted or corrupted mission progress.

Mission timers, lap progression, objective counters and temporary states should follow the intended design rules consistently.

## Design Question

Is respawn intended to be a clean recovery, a time penalty, or a risk/reward mechanic?

QA should confirm this with Design, because the correct result depends on whether the game wants to preserve flow or punish mistakes.

## Red Flags

* Player respawns ahead of the intended checkpoint.
* Player loses lap or objective progress.
* AI gains or loses unfair distance after player respawn.
* Timer keeps running during an unskippable respawn animation, if this is not intended.
* Camera snaps before the player can understand direction.
* Respawn breaks active mission scripting.
* Temporary modifiers persist or disappear inconsistently.

## Data to Capture

* Checkpoint ID.
* Player transform before/after respawn.
* Mission timer before/after respawn.
* Lap/objective state.
* AI positions.
* Active modifiers.
* Video of failure.
* Build and platform.

## Failure Impact

High.

Respawn issues are highly visible and can turn normal failure recovery into frustration, exploits or progression blockers.
