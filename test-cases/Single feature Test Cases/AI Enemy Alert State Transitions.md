# TC-FEAT-002 — AI Enemy Alert State Transitions

**Feature:** AI perception / alert state
**Area:** AI QA, gameplay readability, player trust
**Priority:** High for stealth, action and combat games

## Why I would test this

AI detection is not only about whether the enemy can see the player.

The real question is whether the enemy state transition feels readable and fair: unaware → suspicious → investigating → alerted → searching → returning to normal.

When this breaks, players usually describe the AI as cheating, blind or random. That makes it both a functional QA issue and a design readability issue.

## Test Objective

Verify that enemy AI transitions between alert states consistently, fairly and in a way the player can understand through feedback, behavior and game rules.

## Preconditions

* Enemy AI has multiple alert states.
* Player can enter line of sight, hide, crouch, make noise, attack or distract.
* Detection UI, audio or animation feedback exists.
* Different visibility conditions are available if supported: darkness, cover, foliage, crowd, verticality.

## Test Steps

1. Approach the enemy from outside detection range.
2. Enter line of sight briefly, then break line of sight.
3. Observe whether the enemy enters suspicion or investigation state.
4. Make noise from behind cover.
5. Move behind the enemy without making noise.
6. Attack or distract near the enemy without direct visibility.
7. Trigger full alert, then break line of sight and hide.
8. Observe search behavior and return-to-idle behavior.
9. Repeat in:

   * bright areas;
   * dark areas;
   * cover or foliage;
   * noisy areas;
   * vertical spaces;
   * narrow corridors;
   * open spaces.
10. Compare AI behavior with UI/audio feedback.

## Expected Result

AI state should transition according to clear and consistent rules.

The enemy should not instantly know the player’s exact position unless the game explicitly supports shared knowledge, radio calls, supernatural awareness or another readable design reason.

Suspicion should escalate and decay consistently. Search behavior should feel believable and should not snap unnaturally between complete ignorance and perfect knowledge.

## Design Question

How much information should enemies share?

This needs clarification from Design. A tactical game may allow squad communication. A stealth game may require more localized suspicion and slower information sharing.

## Red Flags

* Enemy detects the player through opaque geometry.
* Enemy loses the player instantly in an unrealistic way.
* Suspicion UI shows “safe” while the enemy is actively tracking.
* One enemy seeing the player alerts the entire map without explanation.
* AI returns to idle while standing next to obvious evidence.
* Enemy knows the player’s exact hiding location after line of sight is broken.
* Detection behaves differently across similar lighting or cover conditions.

## Data to Capture

* AI state before and after trigger.
* Player position.
* Enemy position and facing.
* Visibility/cover status.
* Noise event location.
* Detection meter value.
* AI perception/blackboard data if available.
* Video with debug overlay.

## Failure Impact

Medium to High.

AI alert issues damage perceived fairness. In stealth or tactical games, they can break the core gameplay loop entirely.
