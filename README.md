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
