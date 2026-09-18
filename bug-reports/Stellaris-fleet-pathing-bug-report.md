# Bug Report — Fleet Pathing Ignores Optimal Hyperlane Route Under Active Threat Conditions

**Game:** Stellaris (live public build)
**Category:** Gameplay & Technical QA — pathfinding / state management
**Reporter:** Marco Trapani
**Status:** Open / independently reproduced, not yet submitted to developer

---

## Summary

When a player-controlled fleet is ordered to move to a system exactly one hyperlane hop away, the pathfinder occasionally rejects the direct connection and plots a multi-hop detour through unrelated systems — even when the direct hyperlane is uncontested, undamaged, and was usable moments earlier in the same session. The behavior is intermittent, session-persistent once triggered, and resolves after a save/reload, which points to a stale or corrupted pathing state rather than a deliberate AI routing decision (e.g. gateway/wormhole preference).

This matters beyond convenience: in an active-war state, a fleet forced onto a long detour can arrive too late to intercept a raid, defend a border system, or respond to an emergency evacuation order — directly affecting combat outcomes the player had no way to anticipate or override.

---

## Environment

| Field | Detail |
|---|---|
| Platform | PC (Steam) |
| Game state | Mid-to-late game, active war with a rival empire |
| Galaxy setup | Standard elliptical, default hyperlane density |
| Session length at time of bug | ~3h current session |
| Reproducibility | Intermittent — confirmed across 3 separate sessions, not on-demand |

---

## Steps to Reproduce

1. Enter an active war state with at least one rival empire.
2. Select a fleet stationed in a system with a direct, single-hop hyperlane connection to an adjacent system.
3. Issue a move order to that adjacent system via direct selection (not double-click multi-system routing).
4. Observe the plotted route in the fleet orders UI before confirming movement.
5. Compare against the galaxy map's hyperlane connections for the same two systems.

**Expected result:** Fleet plots the single, direct hyperlane hop.

**Actual result:** Fleet plots a route through 2–4 intermediate systems, in some observed cases doubling back through a system adjacent to the origin, before reaching the stated destination.

**Workaround confirmed:** Saving and reloading the game clears the issue for that fleet until it recurs later in the session.

---

## Evidence Captured

- Screenshot of the galaxy map showing the direct hyperlane between origin and destination.
- Screenshot of the fleet orders panel showing the plotted (incorrect) route, same in-game moment.
- Save file from immediately before issuing the move order.
- Session log / timestamp, cross-referenced with concurrent AI fleet movement in the same systems (a contributing factor observed anecdotally: enemy or third-party fleet presence near the direct hyperlane at the moment of order issuance).

---

## Risk & Impact Assessment

| Category | Assessment |
|---|---|
| **Severity** | Medium — no crash or save corruption, but directly alters combat/strategic outcomes |
| **Reproducibility** | Low-to-medium — intermittent, condition-dependent |
| **Player-facing visibility** | High — visible immediately in the orders UI to any player paying attention to fleet timing |
| **Systemic risk** | Suggests a caching or state-refresh issue in the pathfinding module tied to contested-system flags; worth checking whether the same stale-state pattern affects other systems that key off hyperlane control status (e.g. sector automation, trade route calculation) |

---

## Suggested Isolation Steps

1. Determine whether the trigger correlates with a hostile fleet having *recently* passed through or occupied the direct hyperlane's endpoint system (even if it has since left) — this would point to a stale "contested" flag rather than live contestation.
2. Test whether the bug reproduces in a non-war state with only neutral/monster fleet presence, to isolate whether "war state" itself is a precondition or coincidental.
3. Check whether manually re-selecting the fleet (deselect/reselect) before issuing the order clears the bad path without a full save/reload — this would narrow the issue to UI/selection-state caching rather than the underlying pathfinding calculation.

---

## Methodology Note

This report follows the investigative structure used across this portfolio: reproduce first, isolate variables before proposing root cause, and flag systemic risk beyond the single reported symptom rather than closing the ticket at "it happened once."
