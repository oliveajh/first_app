# Prompt log

## 2026-11-18

1. **Asked:** Set up the project folders, folder READMEs, and `AGENTS.md` layout guidance.
   **Changed:** Added the layout and file-placement rule to `AGENTS.md`. README content was added, then undone by the user; those changes were not restored. No app was built.
   **Worked:** Partially; the layout guidance remains, but the folder READMEs are absent.

2. **Asked:** Confirm the current layout.
   **Changed:** Checked the project tree and reported the missing `00_planning/` folder and READMEs, plus empty top-level placeholders.
   **Worked:** Yes; status was reported accurately.

3. **Asked:** Create `00_planning/`.
   **Changed:** Created the folder.
   **Worked:** Yes; verified it exists.

4. **Asked:** Read the spec, summarize the app, and assess missing items and section order.
   **Changed:** Reviewed `spec.md` and provided a high-level summary and gaps; did not edit the spec.
   **Worked:** Yes.

5. **Asked:** Work only on Inputs; identify unclear or missing inputs and check units, ranges, defaults, and controls.
   **Clarifications:** Set span to 4–40 ft with 10 ft default and advisory warnings; support defaults to simply supported; loads can be upward; use W8x10 as steel default, built-in catalogs, and the previous validation beam as the startup example. Recommended load-entry defaults where the user was unsure.
   **Changed:** Updated only the Inputs section with units, ranges, defaults, and controls.
   **Worked:** Yes.

6. **Asked:** Check the Calculations section equations, units, undefined variables, and missing calculations.
   **Changed:** Reviewed the section and reported unit/definition gaps and missing calculation details; did not edit the spec.
   **Worked:** Yes.

7. **Asked:** Explain what is missing.
   **Changed:** Summarized the missing variables, equations, properties, and check criteria; did not edit the spec.
   **Worked:** Yes.

8. **Asked:** Help define the missing calculation details.
   **Clarifications:** Confirmed linear-elastic Euler–Bernoulli assumptions, inch-based internal units, one simultaneous load case, no automatic self-weight, and L/360 for both support types. Chose limited educational screening, not code-compliant design.
   **Changed:** Updated only the Calculations section with the model, equations, units, checks, and limitations.
   **Worked:** Yes; exact catalog property values and their sources remain to be established.

9. **Asked:** Check whether prompts and questions were being recorded as required by `AGENTS.md`.
   **Changed:** Found `prompts.md` empty and backfilled the visible requests, clarifications, changes, and outcomes.
   **Worked:** Yes; this log now records the visible conversation. Continue appending an entry after each request.

10. **Asked:** Review the Outputs section and suggest what is missing, without building anything.
    **Changed:** Reviewed the current outputs and provided suggestions; did not edit `spec.md` or build the app.
    **Worked:** Yes.

11. **Asked:** Show how the suggested output items could be added.
    **Changed:** Drafted replacement wording for the Outputs section; did not edit `spec.md` or build the app.
    **Worked:** Yes.

12. **Asked:** Apply the proposed Outputs wording to `spec.md`.
    **Changed:** Replaced the Outputs bullets with labeled diagrams, reactions, quantitative check results, warnings, and limitations; no app was built.
    **Worked:** Yes.

13. **Asked:** Suggest a Look and feel plan with a neutral background, editable inputs on the left, and aligned diagrams in a second column.
    **Changed:** Reviewed the current section and prepared proposed wording; did not edit `spec.md` or build the app.
    **Worked:** Yes.

14. **Asked:** Implement the proposed Look and feel plan in the spec.
    **Changed:** Replaced the Look and feel text with the neutral two-column responsive layout and aligned diagram plan; did not build the app.
    **Worked:** Yes.

15. **Asked:** Review the Validation section for missing cases and loopholes, and suggest a plan to address them without editing or building.
    **Changed:** Reviewed the validation case against the rest of the spec and prepared a validation plan; did not edit `spec.md` or build the app.
    **Worked:** Yes.

16. **Asked:** Implement the validation plan.
    **Changed:** Expanded the Validation section with reproducible cases, expected results, boundary checks, tolerances, consistency checks, and catalog-data prerequisites; did not build the app.
    **Worked:** Yes.

17. **Asked:** Expand the Out of scope section with features and checks not included.
    **Changed:** Listed excluded beam/support types, design checks, code compliance, load cases, custom properties, and advanced behavior/loading effects; did not build the app.
    **Worked:** Yes.

18. **Asked:** Read the spec and suggest a 4–6-stage implementation plan starting with a no-calculation layout mockup, with checks for each stage.
    **Changed:** Reviewed the full spec and prepared a six-stage plan; did not edit the spec or write code.
    **Worked:** Yes.

19. **Asked:** Build Stage 1 as a single-file HTML mockup with all inputs, outputs, and plots filled with realistic placeholder values, without calculations.
    **Changed:** Created a static, responsive mockup in `index.html` using inline HTML and CSS; no calculations or build tools.
    **Worked:** Yes; visually verified in the browser.

20. **Asked:** Put the beam and moment/shear diagrams first in the wide-screen right-hand results column.
    **Changed:** Reordered the wide-screen results with diagrams first, kept the summary first on narrow screens, and updated the Look and feel requirement in `spec.md`.
    **Worked:** Yes; verified desktop and mobile order in the browser, with no horizontal overflow at 390 px.

21. **Asked:** Make wide view the default and add a bottom-right mobile/computer icon toggle between wide and stacked layouts.
    **Changed:** Added a wide-default layout switch to `index.html` and updated the Look and feel rules in `spec.md`.
    **Worked:** Implemented, but not browser-verified after the user asked to stop opening the page.

22. **Asked:** Build Stage 2 input controls and validation only, with calculations separated from drawing code.
    **Changed:** Added editable inputs, load-row add/remove controls, material-dependent section fields, and named validation functions. Kept structural results and SVG plots static; no structural calculations were added.
    **Worked:** Static checks passed for unique IDs and validation-function presence. Runtime interaction was not browser-tested at the user's request.

23. **Asked:** Open the page.
    **Changed:** Opened `index.html` in the browser.
    **Worked:** Yes; the Stage 2 page rendered successfully.

24. **Asked:** Straighten the beam/load arrows, make edited inputs update results and the example indicator, and continue to the next build stage.
    **Changed:** Implemented Stage 3 statics in `index.html`: current support reactions, fixed-end moment, maximum moment and location, plus updated beam, shear, and moment diagrams. Rebuilt arrows from vertical line shafts and triangles, corrected reaction-label alignment, and made the example badge reflect whether inputs still match the startup example. Added the six-stage roadmap to `spec.md`.
    **Worked:** Yes; browser-tested the baseline, 10 ft full-span load, partial-span load, midspan point load, cantilever end load, mixed upward/downward loads, invalid inputs, and overflow handling. Expected reactions/moments matched the spec; all inspected arrow shafts were vertical.

25. **Asked:** Check whether maximum deflection and the deflection limit are working.
    **Changed:** Clarified that maximum deflection remains a fixed baseline sample until Stage 4; made the L/360 limit update from the current valid span and clearly labeled the deflection value as not recalculated.
    **Worked:** The limit updates correctly (0.667 in for 20 ft; 0.333 in for 10 ft) and clears for an invalid span. Actual maximum deflection is not implemented yet.

26. **Asked:** Proceed with Stage 4.
    **Changed:** Checked for section-property data in the project; none is present. The attempted external AISC source lookup was declined, so no E or I values were guessed or added.
    **Worked:** Blocked pending trusted E and I values and their source for the section(s) to support.

27. **Asked:** Add an applied load after section 04, let me choose its location, and include it in the calculations. Clarified that this should be a concentrated applied moment.
    **Changed:** Added repeatable signed applied-moment inputs as section 05, with kip-ft magnitude and span position; included moment effects in reactions, shear/moment and deflection responses, bending screening, and the beam/moment diagrams. Updated `spec.md` with the sign convention and validation case.
    **Worked:** Yes; browser-tested clockwise and counterclockwise moments, simply supported and cantilever responses, both span endpoints, multiple moments and removal, invalid positions, and the reference midspan-moment case.

28. **Asked:** Take the next step; interpreted as Stage 6, Integration and hardening.
    **Changed:** Fixed narrow mobile overflow from the long results badge, corrected the view-toggle action label, updated the visible stage to Stage 6, and added the applied-moment deflection reference to `spec.md`.
    **Worked:** Yes; browser-checked baseline, point-load, cantilever, partial-span, mixed-load, and applied-moment cases; span/load boundaries; bending and deflection thresholds; overflow handling; and the mobile layout at 390 px with no horizontal overflow.