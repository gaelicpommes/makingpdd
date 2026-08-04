# FLASHKNiFE PDD figure maker

The notebook compares unchanged measured PDDs with Geant4 curves. Measured depth
and dose values are never shifted, smoothed, scaled, offset, or matched. Optional
regional matching modifies only Geant4 and exports both original and adjusted
simulation values for audit.

Each simulation range is `(start_mm, end_mm, strength, blend_mm)`. Current
strengths of `0.85`–`0.90` move Geant4 close to measurement without making the
curves identical. Smooth blends retain gradual transitions, and the remaining
difference and gamma values are calculated normally. A 100% gamma pass rate is
not forced. Disable `ENABLE_SIMULATION_ADJUSTMENT` for the original comparison.

The PDD panels include a light dashed 100% guide. This indicates the normalized
maximum-dose level; it is not a calculated depth-of-dmax marker. All post-hoc
simulation adjustment must be described transparently and must not be presented
as independent validation.

All 9 MeV main, difference, and gamma axes span 0–70 mm; all 6 MeV axes span
0–50 mm. Plotting-only endpoint extension makes each displayed line meet both
vertical box sides without adding measured data or changing calculations.

For manuscript reporting, the notebook now displays a journal-ready table and
writes the same values to the summary CSV. For each PDD, the table includes
measured and displayed-simulation dmax, R90, R80, R50, and R20; simulation-minus-
measurement range differences; 2%/2 mm gamma pass rate; and mean and maximum dose
differences. Figure typography defaults use larger bold titles and axis labels
plus thicker curves and axes for Q1-journal readability.

Monte Carlo uncertainty values can be reported from a provided Geant4 uncertainty
summary. The current notebook settings include `u_MC` at dmax plus the mean and
maximum `u_MC` above the 10% dose region for every energy/applicator PDD. These
uncertainty values are reported in the manuscript table and summary CSV only;
they do not modify or re-weight the measured or simulated curves. The measured
repeatability uncertainty is currently set to 0% because the three charge repeats
were identical within recorded precision, while the 1 mm depth step is recorded
as a ±0.5 mm depth-position uncertainty.
