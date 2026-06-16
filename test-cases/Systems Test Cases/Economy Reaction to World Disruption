STS-SYS-001 — Economy Reaction to World Disruption

System: Economy, world events, trade routes, player readability
Type: Systemic QA, QA Design, telemetry-driven investigation
Priority: Medium to High depending on game genre

Purpose

Assess whether the economy reacts coherently to a major world disruption such as a siege, blocked route, destroyed village, faction war, resource shortage or scripted crisis.

This is not a test for “prices changed”.
The real test is whether the player can connect the economic change to the world event and make decisions from it.

Systems Involved
Item pricing.
Supply/demand.
Trade routes.
AI merchants or caravans.
Settlement inventory.
Player inventory/trading.
World event state.
UI feedback.
Save/load persistence.
Test Setup

Prepare or identify a location with:

stable baseline economy;
visible resource source nearby;
active trade traffic;
one major disruption event;
player access to buy/sell before and after the event.

Example disruptions:

town siege;
bridge/route blocked;
village raided;
regional war starts;
resource production site disabled;
weather event blocks travel.
Test Flow
Record the baseline state:
key item prices
stock quantities
trade route activity
merchant/caravan visits
local prosperity or equivalent metric
Trigger or observe the disruption.
Track immediate reaction.
Track delayed reaction after several in-game time intervals.
Save/load during the disrupted state.
Resolve the disruption.
Track recovery.
Check whether the player receives enough information to understand the change.
Expected Result

Economic state should move in a coherent direction.

For example:

food should become scarcer during a siege;
route danger should affect deliveries;
local shortages should create visible opportunity;
recovery should take time rather than instantly reset;
UI, NPC dialogue, map state or market data should give the player enough clues.

The economy does not need to expose every formula.
It does need to expose enough cause and effect to be playable.

What I Would Ask Design
Is the economy meant to be simulated, semi-simulated or mostly authored?
Should the player be able to exploit shortages for profit?
How visible should economic causes be?
Is instant recovery acceptable for pacing reasons?
Are shortages meant to punish the player, create opportunity, or both?
Observability Needed
Item price before/during/after event.
Stock quantity.
Source production state.
Trade route status.
Merchant/caravan spawn and arrival logs.
Event timestamp.
Settlement prosperity/security/food state.
Player-facing feedback.
Failure Patterns
Prices change without any readable cause.
Stock resets immediately after save/load.
Trade routes continue normally through blocked or hostile regions.
Shortage exists in numbers but is not communicated.
Economy reacts in the opposite direction from the disruption.
Player cannot make informed decisions without external knowledge.
Risk Assessment

A broken or opaque economy does not always block progress, but it weakens the sense that the world is alive.
For systemic games, that is a design-quality risk, not just a balance issue.
