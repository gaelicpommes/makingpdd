# FLASHKNiFE PDD figure maker

Open `FLASHKNiFE_PDD_figure_maker.ipynb`, edit the settings cell, and run all
cells. The notebook supports two separate, auditable operations on each
measured PDD:

1. a horizontal depth shift; and
2. optional dose adjustments limited to named depth sections.

The original measured values, shifted values, and final adjusted values are
kept in the pointwise CSV output. See the notebook's **How to tune a measured
PDD** section for examples, approximate starter tuples for all six panels, and
reporting cautions. Starter tuples are disabled by default so that opening and
running the notebook never silently edits measured dose values.

## Processing order

The notebook always processes a measured curve in this order:

1. choose the depth shift (automatic optimization or the configured manual value);
2. apply that shift to the measured depth coordinates; and
3. apply enabled section adjustments using those shifted depth coordinates.

Automatic shift optimization compares the simulation with the original measured
doses; section adjustments do not influence the fitted shift. The supplied
starter tuples were estimated from the displayed, already-shifted curves in the
reference plots, but remain inactive until copied into
`MEASURED_SECTION_ADJUSTMENTS`.

Adjacent section corrections are blended with raised-cosine shoulders and are
weight-averaged where they overlap. This keeps the adjusted measured curve
smooth instead of stacking corrections into visible bumps. The figure displays
the difference panels from -5% to +5% and gamma panels from 0 to 1; exported
values are not clipped by these display limits.

## Hidden implementation cell

The long analysis-functions cell is collapsed using Jupyter cell metadata while
remaining fully executable. `Run All` still runs it before the plotting cell.
The editable settings cell stays visible. Expand the cell in JupyterLab or
remove its `jupyter.source_hidden` and `hide-input` metadata to show its source.

## Adjustment tuple reference

Each tuple is `(start_mm, end_mm, scale, offset_pp, blend_mm)`, and applies
`adjusted dose = measured dose * scale + offset_pp` over the shifted-depth
section. Use a scale above `1.0` or a positive offset to raise dose; use a scale
below `1.0` or a negative offset to lower dose. `blend_mm` controls the smooth
transition width. Recommended tuples remain inactive until copied into
`MEASURED_SECTION_ADJUSTMENTS`.

The current starter profiles target the requested shifted-depth regions only:
9 MeV/10 cm at 0–15 and 40–60 mm; 9 MeV/5 cm at 0–15 and 20–50 mm;
6 MeV/10 cm at 0–10 and 30–45 mm; 6 MeV/5 cm at 0–15 and 30–45 mm; and
6 MeV/2 cm at 15–60 mm. The 9 MeV/2 cm profile is empty because no correction
region was requested for that panel.

For residual bumps after that first pass, the notebook also provides
`RECOMMENDED_FINE_TUNE_ADJUSTMENTS`. This is a separate second stage with broad,
low-amplitude corrections and large blend widths over the latest requested
regions. It remains disabled by default and can be copied into
`MEASURED_FINE_TUNE_ADJUSTMENTS` after enabling the main recommendations.

## Direct simulation matching

`ENABLE_SIMULATION_MATCH = True` applies a final, explicit interpolation-based
match after shifting and section edits. At the default strength of `1.0`, each
measured point is moved onto the simulated PDD at the same shifted depth, so the
blue curve is drawn over the orange curve. The legend, panel annotation, summary,
and pointwise audit output identify the result as simulation-matched. This is a
post-hoc visualization/edit and must not be presented as independent validation;
set the flag to `False` for the ordinary measured-versus-simulated comparison.

## Journal figure defaults

The requested publication view uses a dashed blue line without markers for the
measured samples and a solid orange line for the Geant4 curve. Change
`MEASURED_LINEWIDTH` to control the dashed-line thickness.
Simulation matching is disabled by default. Panel shift annotations and coloured
subplot backgrounds are removed, axes and data lines are heavier, and the depth
axis ends at 60 mm without horizontal padding. Difference and gamma are solid
lines without markers. Gamma uses a 0% dose threshold so
it is evaluated through the final measured depth instead of stopping near the
distal falloff; the numerical shift remains available in the audit CSV.

Geant4 input also consists of discrete voxel-centre samples. They are dense, so
the journal default draws them as a solid line without markers. Set
`SHOW_SIMULATION_MARKERS = True` to add sparse simulation dots; use
`SIMULATION_MARKER_EVERY` to change their spacing while retaining the dashed
line for measured observations.

All shared axes use the exact 0–60 mm depth range and disable Matplotlib's
horizontal margin. A curve touches the y-axis when its data contain a value at
0 mm; the notebook does not fabricate or extrapolate missing entrance points
for positively shifted curves.
