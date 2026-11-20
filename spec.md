# Beam Designer

## Purpose
Shear, moment, and deflection diagrams for a beam, with a check of
whether the beam is strong and stiff enough.

## Build stages
1. **Layout mockup:** Single-file responsive page with all planned inputs, outputs, and plots using illustrative values; no calculations. Check that the wide and mobile layouts show all sections and controls.
2. **Inputs and validation:** Editable span, support, section, point-load, uniform-load, and applied-moment controls with validation. Check accepted/rejected boundaries and load-row add/remove behavior.
3. **Beam statics:** Calculate reactions, shear, and moment for point loads, uniform loads, and concentrated applied moments; update the beam/load sketch and shear/moment diagrams from current inputs. Check the baseline and all load validation cases below.
4. **Deflection:** Calculate and plot deflection from section E and I; calculate the L/360 limit and pass/fail from the current span and loads. Check the baseline and point-load formulas, cantilever behavior, and partial-span reference curve.
5. **Material screening:** Calculate maximum bending stress from the current moment and supplied Sx values, compare it with supplied Fb, and show location and pass/fail. Check the baseline and pass/fail boundaries; treat these user-provided values as unverified educational estimates, not code-compliant design.
6. **Integration and hardening:** Verify diagram/output consistency across combined point, uniform, and moment loads; input boundaries; numerical overflow handling; and both wide and mobile layouts across the complete app.

## Inputs
- **Support:** Simply supported or cantilever; default to simply supported. Use a selector. For a cantilever, the fixed end is the left end.
- **Span:** Enter in ft with a numeric field. Default to 10 ft; nominal range is 4–40 ft. Warn outside that range but allow any finite positive span.
- **Point loads:** Enter as repeatable rows with add/remove controls. Each row has a signed magnitude in kip (positive downward, negative upward) and a position in ft from the left end, from 0 to the span. Default to no point loads; a new row starts at 1 kip at midspan. Allow any finite magnitude without a fixed cap.
- **Uniform loads:** Enter as repeatable rows with add/remove controls. Each row has a signed magnitude in kip/ft (positive downward, negative upward) and start/end positions in ft, each from 0 to the span. Allow any finite magnitude without a fixed cap. A new row defaults to 1 kip/ft over the full span.
- **Applied moments:** Enter as repeatable rows with add/remove controls after the uniform-load section. Each row has a signed magnitude in kip-ft (positive clockwise, negative counterclockwise) and a position in ft from the left end, from 0 to the span. Default to no applied moments; a new row starts at 1 kip-ft clockwise at midspan. Allow any finite magnitude without a fixed cap.
- **Beam section:** Use a material selector followed by a section/size selector backed by built-in catalogs for steel W-shapes and lumber. Lumber entries include species and grade so their properties are identified. Default to steel W8x10; require an explicit lumber selection rather than guessing a species or grade. Units do not apply; choices are limited to catalog entries.
- **Startup example:** Load the validation beam as the initial example: simply supported, 20 ft, W12x26, with 1 kip/ft downward over the full span. This is separate from the form defaults above.

## Section properties
The following values were supplied by the user on 2026-11-18 and have not been independently verified. E is in ksi, Ix in in^4, Sx in in^3, and steel weight in lb/ft. Catalog weight is informational only; self-weight is not automatically applied.

| Steel W-shape | E | Ix | Sx | Fb | Weight |
|---|---:|---:|---:|---:|---:|
| W8x10 | 29,000 | 30.8 | 7.81 | 30 | 10 |
| W10x12 | 29,000 | 53.8 | 10.9 | 30 | 12 |
| W12x26 | 29,000 | 204 | 33.4 | 30 | 26 |
| W14x22 | 29,000 | 199 | 29.0 | 30 | 22 |
| W16x26 | 29,000 | 301 | 38.4 | 30 | 26 |
| W18x35 | 29,000 | 510 | 57.6 | 30 | 35 |
| W21x44 | 29,000 | 843 | 81.6 | 30 | 44 |
| W24x55 | 29,000 | 1,350 | 114 | 30 | 55 |

| Douglas Fir-Larch No. 2 | E | Ix | Sx | Fb |
|---|---:|---:|---:|---:|
| 2x8 | 1,600 | 47.63 | 13.14 | 0.900 |
| 2x10 | 1,600 | 98.93 | 21.39 | 0.900 |
| 2x12 | 1,600 | 177.98 | 31.64 | 0.900 |

## Calculations
- Model the beam as prismatic with constant section properties, using linear-elastic, small-deflection Euler–Bernoulli beam theory and strong-axis bending. Apply all entered loads together; do not include self-weight unless entered as a load.
- Calculate support reactions from static equilibrium for either support type, then calculate shear V(x) and moment M(x) along the span. Positive point/uniform loads act downward, positive reactions act upward, positive applied moments act clockwise, and positive internal moment is sagging. Include applied moments in moment equilibrium and reactions; a concentrated applied moment causes a moment-diagram jump of its signed magnitude but does not directly cause a shear jump. Reactions and shear are in kip; calculate statics moments in kip-ft for display and convert them to kip-in when used in section-stress and deflection calculations.
- For a simply supported beam, with signed downward resultant forces P_i at x_i, uniform-load resultants W_j at centroids x_j, span L, and signed clockwise applied moments C_k, calculate R_B = (Σ P_i x_i + Σ W_j x_j + Σ C_k)/L and R_A = Σ P_i + Σ W_j − R_B. For a cantilever, calculate the vertical reaction as the total signed vertical load and the fixed-end reaction moment as the total signed clockwise moment of the applied loads about the fixed end.
- Include the one-sided moment values immediately before and after each applied-moment location when finding maximum absolute moment and drawing the moment diagram. Deflection remains continuous across an applied moment; integrate each piece of the moment curve using its appropriate one-sided endpoint values.
- Calculate deflection v(x) from the beam equation. With downward deflection positive and sagging moment positive, use E I v''(x) = -M(x). Use inches internally for integration: x and L in inches, E in ksi, I in in^4, M in kip-in, and v in inches.
- The section catalog supplies E, I, strong-axis section modulus S, and a screening allowable bending stress F_b for each section. Record the source of these values; use E in ksi, I in in^4, S in in^3, and F_b in ksi.
- Calculate maximum bending stress as fb,max = max_x(|M(x)|)/S, converting moment to kip-in before dividing by S in in^3. Bending passes when fb,max <= F_b. This is an educational screening check, not a code-compliant strength check.
- Calculate maximum absolute deflection as vmax = max_x(|v(x)|). Deflection passes when vmax <= L/360, using the total entered loads for both support types and L in inches.
- These checks do not include buckling or code-required material adjustment factors and must not be presented as code-compliant design.

## Outputs
- Show the beam, its supports, and all point loads, uniform loads, and applied moments labeled with magnitude, direction, and position or extent. Show support reactions, including the fixed-end moment for a cantilever.
- Show shear, moment, and deflection diagrams with labeled axes and units: shear (kip), moment (kip-ft), and deflection (in). Note when the deflection shape is visually exaggerated.
- For the bending check, show the maximum bending stress, its location, the screening allowable, and pass/fail.
- For the deflection check, show the maximum absolute deflection, its location, the L/360 limit, and pass/fail.
- Show applicable warnings and limitations, including when the span is outside the nominal range and that self-weight is excluded unless entered. State that the checks are not code-compliant.

## Look and feel
- Use a neutral background, clear text contrast, and restrained accent colors.
- Default to the wide layout, with all editable inputs in a left-hand panel and results in a wider right-hand column.
- Group inputs by support/span, beam section, point loads, uniform loads, and applied moments (section 05 after uniform loads). Keep load rows easy to add, edit, and remove.
- On wide screens, place the beam/load sketch, shear diagram, moment diagram, and deflection diagram first in the results column, followed by the summary and warnings. Stack the diagrams vertically and align them to the same horizontal span scale.
- Add a fixed bottom-right layout toggle. In the wide view it shows a mobile-device icon and switches to the stacked mobile view; in the mobile view it shows a computer icon and switches back to wide view. Start in wide view on every screen size.
- In the mobile view, stack the input panel above the results, with the summary and warnings before the diagrams.
- Open with the example beam loaded.

## Validation
- Unless stated otherwise, test cases exclude self-weight. For each numeric result, require |actual - reference| <= max(0.001 * |reference|, 0.000001 in the result's unit).
- **Baseline:** 20 ft simply supported W12x26; no point loads; 1 kip/ft downward uniformly over the full span. Expected reactions are 10 kip at each support; shear has magnitude 10 kip at each end and crosses zero at midspan; maximum moment is 50.0 kip-ft at midspan. Maximum deflection is 0.609 in at midspan using the supplied E and I; the L/360 limit is 0.667 in, so deflection passes. Maximum bending stress is 600 kip-in / 33.4 in^3 = 17.964 ksi using the supplied Sx; it passes the supplied 30 ksi Fb.
- **Simply supported point load:** 20 ft span with a 10 kip downward point load at midspan and no other loads. Expected reactions are 5 kip each and maximum moment is 50 kip-ft at midspan. Maximum deflection is P L^3 / (48 E I), using consistent inch units.
- **Cantilever end load:** 10 ft cantilever with a 1 kip downward point load at the free end and no other loads. Expected fixed-end reaction is 1 kip upward, fixed-end moment magnitude is 10 kip-ft, and maximum deflection magnitude is P L^3 / (3 E I) at the free end.
- **Partial-span uniform load:** 20 ft simply supported span with 1 kip/ft downward from 0 to 10 ft and no other loads. Expected reactions are 7.5 kip at the left support and 2.5 kip at the right; maximum moment is 28.125 kip-ft at 7.5 ft. Verify the deflection curve against an independent beam-equation reference using the catalog E and I.
- **Mixed load directions:** 20 ft simply supported span with 1 kip/ft downward over the full span and a 2 kip upward point load at midspan. Expected reactions are 9 kip each. The shear diagram must show a 2 kip upward jump at midspan; the moment has equal local maxima of 40.5 kip-ft at 9 ft and 11 ft.
- **Applied-moment case:** 20 ft simply supported span with a 10 kip-ft clockwise applied moment at midspan and no other loads. Expected reactions are 0.5 kip downward at the left and 0.5 kip upward at the right; shear remains constant at -0.5 kip with no jump at midspan. The moment diagram is -5 kip-ft immediately before the moment and +5 kip-ft immediately after it, returning to zero at the right support. Maximum absolute moment is 5 kip-ft at the moment location. With W12x26, the maximum absolute deflection is approximately 0.00937 in at 5.774 ft and 14.226 ft; deflection remains continuous at midspan.
- **Input and check boundaries:** Spans of 4 and 40 ft are valid without a range warning; positive spans outside that range calculate with a warning; zero or negative spans are rejected. Load and applied-moment positions at 0 and L are valid, while positions outside the span are rejected. Uniform-load start/end positions must satisfy 0 <= start < end <= L. Check bending and deflection just below, exactly at, and just above their limits; equality passes because the comparisons use <=. Test very large finite loads and moments that cause non-finite intermediate or final results; report a calculation error rather than showing misleading diagrams or pass/fail results.
- **Consistency and catalog checks:** Verify force and moment equilibrium, expected shear jumps at point loads, no shear jump for an applied moment, moment-diagram jumps at applied moments, and agreement between diagram extrema and reported checks. Check deflection against supplied E and I for the W12x26 baseline, the applied-moment case, and at least one lumber entry. Check bending stress against supplied Sx and Fb for the baseline, a lumber section, and just-below/equal/just-above threshold cases. The supplied E, I, S, F_b, and weight values remain unverified against independent catalog sources.

## Out of scope
- Continuous or multi-span beams and support conditions other than simply supported beams and cantilevers.
- Buckling, including lateral-torsional and local buckling; weak-axis or biaxial bending; and strength checks for shear, bearing, connections, or supports.
- Code-compliant design, certification, and code-required material adjustment factors.
- Automatic self-weight estimation and multiple load cases or load combinations. All entered loads form one case; self-weight must be entered as a load if it is to be included.
- Custom sections or user-defined material properties; only entries in the built-in steel W-shape and lumber catalogs are supported.
- Non-prismatic beams, nonlinear behavior, large-deflection or shear-deformation effects, and dynamic, impact, fatigue, or thermal loading.
