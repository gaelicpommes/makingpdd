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
