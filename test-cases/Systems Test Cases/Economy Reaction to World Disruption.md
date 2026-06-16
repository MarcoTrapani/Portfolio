# STS-SYS-001 — Economy Reaction to World Disruption

**System:** Economy, world events, trade routes, player readability
**Area:** Systemic QA, QA Design, telemetry-driven investigation
**Priority:** Medium to High, depending on genre

## Purpose

Assess whether the in-game economy reacts coherently to a major world disruption such as a siege, blocked route, destroyed village, faction war, resource shortage or scripted crisis.

This is not just a test for “prices changed”.

The real question is whether the player can connect the economic change to the world event and make meaningful decisions from it.

## Systems Involved

* Item pricing.
* Supply and demand.
* Trade routes.
* AI merchants, caravans or delivery agents.
* Settlement / shop inventory.
* Player trading.
* World event state.
* UI feedback.
* Save/load persistence.

## Test Setup

Prepare or identify a location with:

* stable baseline economy;
* visible resource source nearby;
* active trade traffic;
* one major disruption event;
* player access to buy/sell before and after the event.

Example disruptions:

* town siege;
* bridge or route blocked;
* village raided;
* regional war starts;
* resource production site disabled;
* weather event blocks travel;
* scripted crisis temporarily reduces supply.

## Test Flow

1. Record the baseline state:

   * key item prices;
   * stock quantities;
   * trade route activity;
   * merchant/caravan visits;
   * local prosperity, security or equivalent metric.
2. Trigger or observe the disruption.
3. Track the immediate economic reaction.
4. Track delayed reaction after several in-game time intervals.
5. Save/load during the disrupted state.
6. Resolve the disruption.
7. Track recovery.
8. Check whether the player receives enough information to understand the change.

## Expected Result

Economic state should move in a coherent direction.

For example:

* food should become scarcer during a siege;
* route danger should affect deliveries;
* local shortages should create visible trading opportunities;
* recovery should take time rather than instantly reset;
* UI, NPC dialogue, map state or market data should give the player enough clues.

The economy does not need to expose every formula.

It does need to expose enough cause and effect to be playable.

## Questions I Would Ask Design

* Is the economy meant to be simulated, semi-simulated or mostly authored?
* Should the player be able to exploit shortages for profit?
* How visible should economic causes be?
* Is instant recovery acceptable for pacing reasons?
* Are shortages meant to punish the player, create opportunity, or both?
* Should the disruption affect only local prices or wider regional trade?

## Observability Needed

* Item price before/during/after event.
* Stock quantity.
* Source production state.
* Trade route status.
* Merchant/caravan spawn and arrival logs.
* Event timestamp.
* Settlement prosperity/security/food state.
* Player-facing feedback.
* Save/load state comparison.

## Failure Patterns

* Prices change without any readable cause.
* Stock resets immediately after save/load.
* Trade routes continue normally through blocked or hostile regions.
* Shortage exists numerically but is not communicated.
* Economy reacts in the opposite direction from the disruption.
* Player cannot make informed decisions without external knowledge.
* Recovery happens instantly despite the disruption still being active.
* UI communicates scarcity, but shop inventory remains unchanged.

## Risk Assessment

A broken or opaque economy does not always block progression, but it weakens the sense that the world is alive.

For systemic games, this is a design-quality risk, not just a balance issue.
