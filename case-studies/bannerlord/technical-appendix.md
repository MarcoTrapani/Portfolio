# Extended Technical Appendix

> This appendix expands the main Bannerlord case study with Technical QA artifacts.
>
> This appendix is intended as a practical Technical QA artifact for analyzing systemic sandbox risks, long-run campaign stability, state persistence and player-facing readability.
>
> [Back to Main Case Study](index.md)

---

## Technical QA Framing

In a systemic sandbox, QA also means validating simulation integrity. State changes must be processed correctly, persist after save/load, interact without corrupting long-term progression, and remain understandable to the player.

Many *Bannerlord* systems can pass isolated feature testing while still creating problems once connected to the broader campaign simulation:

* battles can resolve correctly
* armies can move
* settlements can change ownership
* lords can be captured
* prices can fluctuate
* kingdoms can declare war
* AI parties can choose targets

The real QA challenge appears when these systems interact.

A battle result may affect army strength. Army strength may affect war score. War score may affect peace proposals. Peace may affect tribute. Tribute may affect economy. Economy may affect recruitment. Recruitment may affect future army recovery. Army recovery may affect whether the player perceives victory as meaningful.

This is the core QA problem of systemic games: the issue is not only whether each system works: the issue is whether the world still makes sense after those systems collide.

---

## Bug Taxonomy for Systemic Sandbox QA

Not every issue in a sandbox is a bug in the strict functional sense.

In a systemic sandbox, issues can fall across several categories: functional defects, persistence failures, systemic breakdowns, balance problems, exploits, UX/readability issues, performance risks and regressions introduced by interconnected changes.

| Issue Type           | Example in a Sandbox Context                                       | QA Concern             |
| -------------------- | ------------------------------------------------------------------ | ---------------------- |
| Functional bug       | A lord is duplicated after reload                                  | Broken state integrity |
| Persistence bug      | A captured noble does not remain captured after save/load          | Loss of consequence    |
| Systemic bug         | War ends immediately after declaration due to scoring issue        | Campaign logic failure |
| Balance issue        | Defeated factions recover full army strength too quickly           | Late-game grind        |
| Exploit              | Player manipulates prisoner release, tribute or recruitment loops  | Degenerate strategy    |
| UX/readability issue | Player cannot understand why tribute is demanded despite victories | Loss of trust          |
| Performance issue    | Siege battle frame time spikes due to AI/pathfinding load          | Playability risk       |
| Regression issue     | A diplomacy patch breaks alliance behavior in old saves            | Patch stability risk   |

This taxonomy matters because systemic sandbox issues often sit between classic bug categories.

For example “multi-front wars feel arbitrary” is not a classic bug report: it may involve diplomacy scoring, faction strength evaluation, map borders, tribute logic, AI aggression, player feedback and late-game pacing.

The Technical QA task is to transform vague player frustration into testable hypotheses, observable variables and reproducible investigation paths.

---

## Observability Requirements

A QA approach to Bannerlord would require observability into the campaign simulation, especially around diplomacy, army creation, prisoner state, economy and AI decision-making.

In a systemic sandbox, many issues are not visible as immediate failures. They become testable only when enough simulation state is exposed to explain why a specific outcome occurred.

Example: if a kingdom declares war, the player may only see the final event. But QA needs to understand the decision context:

* relative kingdom strength
* current wars
* border exposure
* recent raids
* recent sieges
* tribute expectations
* alliances
* diplomatic relations
* army availability
* economy
* previous war duration
* AI strategic goals

Without this information, a bug report risks staying vague: “The kingdom declared war for no reason.”

With observability, that same issue can become testable:

“The kingdom declared war despite low strength, active two-front conflict, poor economy, no border advantage and negative expected tribute. Decision score appears inconsistent with intended risk evaluation.”

Useful debug data for systemic QA would include:

| Category    | Useful Debug Data                                                    |
| ----------- | -------------------------------------------------------------------- |
| Diplomacy   | War reason scores, peace logic, tribute calculations                 |
| Military    | Army creation/disband logs, noble availability, prisoner state       |
| Economy     | Economy snapshots, caravan routes, trade agreement state             |
| World State | Settlement ownership history, siege state history                    |
| Validation  | Battle result snapshots, save-state comparison, performance captures |

The goal is not to expose every internal variable to the player. The goal is to give QA enough evidence to classify the issue correctly: functional defect, balance problem, AI decision issue, UX/readability gap or intended behavior.

---

## Performance and Stability Risks

Large-scale battles are one of *Bannerlord*’s most distinctive features. They are also one of the most important Technical QA risk areas.

A large battle is not only a combat design challenge: it is a performance, AI, animation, pathfinding, collision, camera, audio and combat-readability challenge happening at the same time.

Risk areas include:

* frame time spikes during reinforcement waves
* CPU spikes during AI decision bursts
* pathfinding failures in siege chokepoints
* cavalry collision issues during dense engagements
* reinforcement spawn spikes
* ranged unit targeting overhead
* animation and ragdoll load
* memory growth during long sessions
* load time increases in advanced campaigns
* save file growth
* crash risk after long play sessions
* stutter when campaign simulation updates during transitions
* performance differences between open-field battles and siege scenes

Useful Technical QA checks would include:

* predefined battle benchmarks
* siege benchmark scenarios
* late-game save performance comparison
* long-session soak testing
* memory tracking over multi-hour sessions
* frame time capture during reinforcement waves
* CPU/GPU bottleneck analysis
* crash log classification
* performance regression comparison across builds

Performance testing should not only ask: “Does the battle run?”

It should ask: “Does the battle remain playable, readable and stable under the conditions that the sandbox naturally creates?”

---

## Reproducibility Challenges

Many of the most important sandbox issues are difficult to reproduce because they emerge from accumulated state.

A bug such as “lord duplicated after reload” may be relatively straightforward to isolate.

A complaint such as “late-game wars feel arbitrary” is more difficult to isolate because it may depend on dozens of variables:

* campaign day
* kingdom strength
* number of active wars
* border proximity
* current tribute values
* recent raids
* recent settlement losses
* noble availability
* captured lords
* alliances
* economy
* player clan tier
* previous war duration
* AI aggression settings
* save history
* recent patch changes

The QA task is to turn that vague experience into a controlled investigation.

Example transformation:

**Player feedback:**
“Lategame wars feel random and only exist to prolong the campaign.”

**QA hypothesis:**
“War declaration scoring may be over-prioritizing opportunity or aggression while under-prioritizing current war load, recent losses, economic state or player-facing explanation.”

**Data needed:**

* war declaration timestamp
* declaring kingdom strength
* target kingdom strength
* current wars for both kingdoms
* recent casualties
* recent raids
* tribute estimates
* active alliances
* border settlement count
* AI decision score breakdown
* player-facing notification

This is where QA adds value: not by accepting player perception as proof, but by using it as a signal for testable investigation.

---

## Automation Candidates

Even if final quality judgment requires interpretation, automated checks could help detect systemic regressions earlier.

### Automated Campaign Smoke Test

1) start or load a controlled campaign
2) simulate a defined number of in-game days
3) check for crashes
4) verify valid kingdoms, settlements and parties

Fail the test if negative values appear in protected state variables, if settlements have invalid or null ownership, or if unique hero IDs are duplicated/missing after simulation.

### Automated Save/Load Integrity Test

Create or load a controlled world state, save, reload, then diff key state variables: prisoner ownership, settlement ownership, active wars, army composition, relations and party locations.

### Automated Battle Benchmark

* launch a predefined large battle
* capture FPS, frame time and memory usage
* monitor crashes and script errors
* compare performance against previous builds

### Legacy Save Regression Test

* load representative saves from previous versions
* validate settlements, wars, characters, quests and economy state
* check for broken diplomacy or invalid ownership
* run a short simulation after load to detect delayed failures

### Economy Sanity Test

* simulate siege, war or trade disruption
* capture price and availability changes
* verify values remain within expected ranges
* detect extreme outliers or broken trade behavior

Automation would be especially useful for repetitive state validation, performance baselines and regression checks. 

---

## Detailed Technical Test Scenarios

### Scenario 1 — Lord Captured After Major Battle

**Risk Type:** Save/load persistence, campaign state integrity

**Preconditions:**

Enemy army contains at least five noble heroes. Player has enough prisoner capacity to capture at least two enemy lords. Battle takes place during an active war, with no peace event triggered before saving.

**Steps:**

1. Complete battle.
2. Capture enemy lords.
3. Save immediately after battle resolution.
4. Reload the save.
5. Advance seven in-game days.
6. Check prisoner state, lord location, enemy army availability and war progress.

**Data to Capture:**

* captured lord IDs
* prisoner owner
* party location
* enemy available noble count
* enemy army strength before/after
* war score before/after
* save/load timestamp

**Expected Result:**

Captured lords remain in valid prisoner state. No duplicated heroes occur. Enemy army availability reflects captured nobles. War progress updates consistently.

**Failure Impact:**

Major loss of player trust, battle consequences appear invalidated.

---

### Scenario 2 — Recently Conquered Settlement Under Pressure

**Risk Type:** AI target selection, settlement state, player readability

**Preconditions:**

* Player conquers a town
* Town has low loyalty, low food and depleted garrison
* Enemy faction still has nearby military capacity

**Steps:**

1. Capture the settlement
2. Save after ownership change
3. Observe enemy and allied AI behavior for several in-game days
4. Track siege attempts, defense response, food, loyalty and militia changes
5. Reload and verify ownership/state persistence

**Data to Capture:**

* settlement owner before/after
* food, loyalty, security and militia values
* enemy party positions
* allied party positions
* siege decision logs
* garrison strength
* player notifications

**Expected Result:**

Enemy response should be consistent with nearby military capacity, target value and distance. Allied AI should evaluate the recently conquered settlement as a defensive priority when its food, loyalty and garrison are critically low. Player notifications should clearly communicate loyalty, food, security and siege risk.

**Failure Impact:**

Conquest feels unstable, arbitrary or impossible to consolidate.

---

### Scenario 3 — Multi-Front War Escalation

**Risk Type:** Diplomacy logic, late-game pacing, player trust

**Preconditions:**

* Player kingdom is already at war with one major faction
* Player kingdom has border exposure to at least two other factions
* Mid-late campaign state

**Steps:**

1. Load the late-game save
2. Track all active wars and diplomatic values
3. Advance campaign time until a new war declaration occurs
4. Capture declaration context and decision variables
5. Repeat across multiple saves or controlled states

**Data to Capture:**

* active wars
* relative kingdom strength
* recent raids/sieges
* border proximity
* tribute estimates
* alliances
* recent peace agreements
* war declaration reason score
* player-facing explanation

**Expected Result:**

Additional wars should be explainable through relations, borders, strength, alliances, opportunity or previous conflict. The declaration should have a traceable decision score and player-facing feedback sufficient to distinguish it from purely anti-player pressure.

**Failure Impact:**

Late-game pressure becomes perceived as artificial prolonging rather than emergent strategy.

---

### Scenario 4 — Economic Disruption After Siege

**Risk Type:** Economy simulation, trade readability, UI feedback

**Preconditions:**

* Town has recently been under siege
* Food availability is low
* Nearby villages and trade routes are active

**Steps:**

1. Track town food availability before siege
2. Observe siege and post-siege state
3. Track food prices, caravan traffic and village deliveries
4. Test player trade opportunities
5. Observe recovery over multiple in-game days

**Data to Capture:**

* item prices before/after
* food stock
* village delivery events
* caravan visits
* town prosperity
* player notifications
* route safety

**Expected Result:**

Economic changes should remain within expected ranges and correlate with visible world events: siege duration, food stock, village deliveries, caravan traffic and route safety.

**Failure Impact:**

Economy feels opaque or disconnected from world events.

---

### Scenario 5 — Defeated Faction Recovery

**Risk Type:** Anti-snowball balance, army regeneration, consequence quality

**Preconditions:**

* Enemy faction loses several armies
* Multiple lords are captured
* Several settlements are taken
* Player kingdom is in a dominant military position

**Steps:**

1. Capture enemy army strength before major defeat
2. Defeat enemy army and capture lords
3. Track enemy party and army formation over 14–30 in-game days
4. Monitor recruitment sources, lord availability and settlement ownership
5. Compare recovery speed against available resources

**Data to Capture:**

* enemy army strength before/after
* number of captured lords
* number of available lords
* settlement count
* recruitment activity
* new army creation timestamp
* troop quality
* war score
* tribute proposals

**Expected Result:**

Recovery should be possible but believable, tied to available lords, settlements, manpower, economy and recent losses.

**Failure Impact:**

Player victories feel strategically meaningless; late-game becomes repetitive.

---

## Example Bug Report

**Title:** Enemy faction rapidly reforms major army despite multiple captured lords and recent settlement losses

**Environment:**
Single-player campaign, late-game, player kingdom active, 900+ campaign days, multiple active wars.
Build: [insert build/version]
Platform: [insert platform]
Save type: [new campaign / legacy save / modded-unmodded]

**Observed Result:**
After losing a major army and multiple nobles, the enemy faction forms a new large army within [X] in-game days. The new army appears disproportionate to its available lords, settlements and recent losses, making the previous victory feel strategically non-persistent.

**Expected Result:**
Enemy recovery should be proportional to available lords, settlements, recruitment capacity, economy and recent losses. If rapid recovery is intended, the game should communicate why the faction is still capable of fielding a large force.

**Reproduction Steps:**

1. Load late-game save with player kingdom active
2. Engage and defeat enemy army
3. Capture multiple enemy lords
4. Save after battle resolution
5. Advance campaign time for 7–14 in-game days
6. Observe enemy army creation and troop quality
7. Compare army recovery against available lords, settlements and recent losses

**Data Needed:**

* captured lord IDs
* prisoner states
* enemy available lord count
* enemy settlement count
* enemy recruitment events
* army creation logs
* war score
* tribute calculations
* party strength before/after
* save/load checks

**Impact:**
Severity: High. The issue can reduce perceived consequence quality, weaken player agency, invalidate major battle outcomes and contribute to late-game war fatigue.

**QA Notes:**
This may not be a strict functional bug. It may be a balance issue, AI recovery issue, missing player feedback issue or intended anti-snowball behavior that lacks readability. Further validation requires access to army creation rules, faction recovery parameters and war/diplomacy scoring.

---

## Full Risk Checklist

| Area                      | Risk                                          | Player Impact                           | QA Focus                                                   |
| ------------------------- | --------------------------------------------- | --------------------------------------- | ---------------------------------------------------------- |
| War/peace logic           | Wars feel arbitrary or too frequent           | Loss of agency, frustration             | Long-run campaign scenarios, decision logging              |
| Battle result persistence | Consequences are not absorbed by the campaign | Victories feel temporary                | Save/load, state validation, prisoner/army checks          |
| AI recovery               | Defeated factions recover too quickly         | Late-game grind                         | Balance testing, telemetry, army regeneration analysis     |
| Economy                   | Prices and shortages are opaque               | Less intentional decision-making        | Trade, food, caravan and siege testing                     |
| Campaign AI               | Poor target selection or passive defense      | Strategic disbelief                     | Army movement, siege response, food/strength evaluation    |
| Battle AI                 | Units behave incoherently                     | Loss of tactical readability            | Formation, pathfinding, cavalry / ranged behavior          |
| Performance               | Large battles or sieges degrade frame time    | Reduced playability                     | Benchmarks, profiling, long-session tests                  |
| Save compatibility        | Old saves break after systemic changes        | Broken progression, player trust damage | Legacy save regression                                     |
| Late-game pacing          | Conflict becomes repetitive                   | Burnout, loss of meaning                | Multi-front war and player kingdom testing                 |
| Exploits                  | Dominant strategies bypass intended challenge | Broken progression                      | Economy, prisoner, diplomacy and recruitment abuse testing |

---

## References

[References](references.md)
