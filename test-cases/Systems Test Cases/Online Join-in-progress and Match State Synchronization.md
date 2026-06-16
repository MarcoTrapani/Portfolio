# STS-SYS-001 — Online Join-in-Progress and Match State Synchronization

**System:** Multiplayer session state / replication / join-in-progress
**Area:** Technical QA, network QA, gameplay integrity
**Priority:** High for online games

## Purpose

Assess whether a player joining an already active match receives the correct current state of the game.

Join-in-progress issues are risky because the joining player may enter a technically valid session while seeing a different version of the match from everyone else.

This can look like a UI bug, spawn bug, objective bug, scoreboard bug or replication issue. The real problem is that clients no longer agree on the authoritative match state.

## Systems Involved

* Session management.
* Spawn logic.
* Player replication.
* Objective replication.
* Scoreboard.
* Team assignment.
* Inventory/loadout.
* Match timer.
* UI state.
* Respawn/spectator flow.
* Destroyed, collected or interacted world objects.

## Test Setup

Prepare an active multiplayer match with:

* at least two players already in session;
* score or objective progress already changed;
* one or more players dead, respawning or spectating;
* active pickups, destroyed objects or captured objectives;
* realistic network conditions, with latency or packet loss simulation if available.

## Test Flow

1. Start a multiplayer match with at least two players.
2. Progress the match beyond its initial state.
3. Have a new client join during active gameplay.
4. Check spawn location, team assignment, loadout and UI.
5. Compare match state between server/host, existing clients and joining client.
6. Repeat join-in-progress during:

   * pre-match countdown;
   * active objective phase;
   * respawn wave;
   * overtime;
   * round transition;
   * end-of-round summary;
   * host migration, if supported.
7. Disconnect and reconnect the same client.
8. Repeat under latency or packet-loss simulation if available.

## Expected Result

The joining player should receive the current authoritative state, not the default match state.

The joining client should correctly display:

* match timer;
* score;
* objective state;
* team assignment;
* alive/dead players;
* destroyed or collected objects;
* available spawn points;
* current round phase;
* correct UI prompts.

Existing players should not experience state rollback, score changes, objective reset or gameplay interruption when a new player joins.

## Questions I Would Ask Dev

* Which state is sent as the initial replication payload?
* Which state is event-based only and may be missed by late joiners?
* Are destroyed or collected objects replicated as current state, or only as past events?
* Is the scoreboard server-authoritative?
* How are reconnecting players identified?
* Can reconnecting players duplicate character/session data?

## Observability Needed

* Server state snapshot.
* Client state snapshot.
* Replication logs.
* Join timestamp.
* Player ID/session ID.
* Objective state.
* Scoreboard state.
* Spawn selection reason.
* Network conditions.

## Failure Patterns

* Late joiner sees already-collected items as still available.
* Objective appears neutral to joining player but captured to existing players.
* Joining player spawns in an invalid or unsafe location.
* Scoreboard resets locally.
* Timer desyncs.
* Reconnecting player duplicates.
* Existing clients receive a state rollback when someone joins.
* UI shows default match instructions instead of current objective state.

## Risk Assessment

High.

A join-in-progress failure can compromise the entire match state. Even if the server remains authoritative, the player experience can become unreliable if one client sees outdated or incorrect information.
