# FLASHKNiFE PDD figure maker

The notebook compares unchanged measured PDDs with Geant4 curves. Measured depth
and dose values are never shifted, smoothed, scaled, offset, or matched. Optional
regional matching modifies only Geant4 and exports both original and adjusted
simulation values for audit.

Set each entry in `SIMULATION_MATCH_SCALES` independently from `0.0` (original
simulation) to `1.0` (full configured regional adjustment). The dictionary has a
separate entry for every energy/applicator PDD combination. Each simulation range
is `(start_mm, end_mm, strength, blend_mm)`, so individual regions remain
tunable. Current strengths of `0.85`–`0.95` move Geant4 close to measurement without
making the curves identical. Smooth blends retain gradual transitions, and the remaining
difference and gamma values are calculated normally. A 100% gamma pass rate is
not forced. Disable `ENABLE_SIMULATION_ADJUSTMENT` for the original comparison.

The PDD panels omit the former 100% guide and overall figure title for a cleaner
export. Every PDD panel has a top-right “Measured” / “Simulated” legend. All post-hoc
simulation adjustment must be described transparently and must not be presented
as independent validation.

All 9 MeV main, difference, and gamma axes span 0–70 mm; all 6 MeV axes span
0–50 mm. Plotting-only endpoint extension makes each displayed line meet both
vertical box sides without adding measured data or changing calculations.

For manuscript reporting, the notebook now displays a journal-ready table and
writes both displayed tables to separate CSV files: the manuscript-formatted
journal table (`FLASHKNiFE_PDD_journal_table.csv`) and the complete analysis
summary (`FLASHKNiFE_PDD_comparison_summary.csv`). For each PDD, the table includes
measured and displayed-simulation dmax, R90, R80, R50, and R20; simulation-minus-
measurement range differences; 2%/2 mm gamma pass rate; and mean and maximum dose
differences. Figure typography defaults use larger bold titles and axis labels
plus thicker curves and axes for Q1-journal readability. Difference and gamma
panels preserve every raw calculated interior sample—including its noise—without
smoothing, denoising, filtering, or interior resampling. Plotting-only endpoint extension makes both lines touch the vertical axes. Gamma ticks are
spaced by 0.5 and the plot omits pass-rate annotations.

Monte Carlo uncertainty values can be reported from a provided Geant4 uncertainty
summary. The current notebook settings include `u_MC` at dmax plus the mean and
maximum `u_MC` above the 10% dose region for every energy/applicator PDD. These
uncertainty values are reported in the manuscript table and summary CSV only;
they do not modify or re-weight the measured or simulated curves. The measured
repeatability uncertainty is currently set to 0% because the three charge repeats
were identical within recorded precision, while the 1 mm depth step is recorded
as a ±0.5 mm depth-position uncertainty.

## Journal comparison method

The reporting block uses the adjusted simulation as the evaluated PDD and the
measurement as reference. Dose difference is reported explicitly as simulated
minus measured in percentage points. Gamma is reported as **1D global 2%/2 mm**
with a **10% measured-reference threshold**, a 0.025 mm search step, and a pass
definition of gamma <= 1. The compact gamma panels use a 0--1 y-axis; nominal
failures remain in the CSV/table and appear as upward triangles at the top edge.

Dose-difference plots can display a `k=2` expanded uncertainty band. The available
budget combines measured repeatability, depth-resolution uncertainty propagated
through the unsmoothed measured gradient, and the supplied Geant4 statistical
uncertainty. Gamma plots and tables include a separate one-standard-uncertainty
sensitivity envelope/range; uncertainty does not relax or redefine the 2%/2 mm
acceptance criterion. The pointwise difference and gamma values and their
uncertainty results are saved to CSV with the plots and tables. These are a
partial uncertainty budget based on the inputs currently available in the
notebook; additional detector calibration and setup components should be added
when available.

## Raw-point visibility and uncertainty shading

The translucent green dose-difference region and pink gamma region are the
calculated uncertainty/sensitivity envelopes, not plot borders. They are hidden
by default (`SHOW_UNCERTAINTY_BANDS = False`) while all uncertainty columns remain
in the saved tables and pointwise CSV files. Set the flag to `True` to show them.
Small markers show every actual difference and gamma sample; the lines receive no
smoothing or denoising. A visually smooth curve therefore means the underlying
adjusted data are smooth—it is not evidence that the plotting code removed noise.
Using a smaller per-case `SIMULATION_MATCH_SCALES` value retains more of the
original simulation-to-measurement residual, but the notebook never fabricates
noise or spikes.

## Data integrity

The current per-case scale and range defaults match the configured manuscript
comparison. The notebook never injects artificial spikes or noise into measured,
simulated, difference, gamma, table, or CSV values. Lower match scales retain
more genuine residual structure from the supplied curves; synthetic perturbations
must not be represented as measured or Monte Carlo results.

## Compact gamma and uncertainty presentation

The manuscript figure hides the light uncertainty/sensitivity bands by default to
reduce clutter; the calculated uncertainty values remain in the saved tables and
CSVs. Gamma panels use a focused 0--1 scale so changes within the passing range
are more visible. Any nominal gamma value above 1 is retained numerically and
marked with an upward triangle at the upper boundary, preventing silent clipping.
After changing these settings, rerun the settings and figure cells (or use **Run
All**) to replace any previously rendered notebook image that still shows bands
or a 1.5 gamma limit.
