# 🐞 Compliance Report – PlayStation TRC Submission Failure: Save Data Corruption on Suspend/Resume

> This report focuses on a **platform compliance issue** identified during PlayStation TRC testing, highlighting potential certification blockers and impact on **player experience and submission approval**.

---

## Summary
The game fails TRC compliance due to **save data corruption** when the console is suspended and resumed. This occurs in multiple game states, potentially causing player progress loss and rejection during PlayStation submission.

---

## Environment
- **Game Build:** v0.8.12 (Internal QA Build)
- **Platform:** PlayStation 5, PLaystation 5 Pro
- **Firmware:** 23.02-04
- **Account State:** Any save slot (multiple tested)
- **TRC Test Cases:** SYS_02 (Save/Load), SYS_03 (Suspend/Resume)
- **Repro Rate:** 80% (occurs in mid-game and during cutscene transitions)

---

## Preconditions
- Player must have an active save file.
- Game must be running in **suspendable mode** (not in main menu).
- Auto-save or manual save enabled.

---

## Steps to Reproduce
1. Launch the game and load a save slot.
2. Play until auto-save triggers, or manually save.
3. Suspend the console (PS button → Enter Rest Mode).
4. Resume the console.
5. Reload the same save slot.

---

## Expected Result
- Save data remains intact after suspend/resume.
- Game resumes exactly where it left off.
- Submission should pass TRC SYS_02 / SYS_03 tests without errors.

---

## Actual Result
- In 80% of attempts, save data is corrupted.
- Players lose progress since last save point.
- Game may crash when attempting to load the corrupted save.
- TRC automated checks would fail.

---

## Impact

### Severity
**Critical – Submission Blocking**

### Player Impact
- Potential permanent loss of progress.
- Frustration leading to negative perception and retention issues.

### Compliance Impact
- TRC SYS_02 / SYS_03 failure prevents submission approval.
- Requires hotfix before release or certification pass.

---

## Root Cause Analysis (Hypothesis)
- Suspend/resume callbacks do not properly serialize ongoing transactions.
- Auto-save and manual save systems are not synchronized with platform suspend events.
- Data may be partially written during suspend, leading to corruption.

---

## Suggested Fix / Design Considerations
- Implement safe-state serialization before suspend.
- Ensure suspend/resume events trigger save commit and file validation.
- Add automated TRC regression test for multiple save slots.
- Consider warning the player if suspend occurs during an active save.

---

## Attachments
- Video capture demonstrating suspend/resume crash.
- Example of corrupted save file.
- PS5 debug log showing save write error.
- TRC test case results screenshot.

---

## Notes
This issue demonstrates the importance of **platform compliance testing**. While the bug is not a gameplay exploit, it **blocks submission** and negatively affects **player trust**. Early detection allows a smoother submission process and reduces post-launch hotfix pressure.

---

## Tags
`TRC Compliance` `Save System` `Submission Blocking` `High Priority`
