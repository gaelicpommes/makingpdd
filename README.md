# FLASHKNiFE PDD figure maker

The notebook compares unchanged measured PDDs with Geant4 curves. Measured depth
and dose values are never shifted, smoothed, scaled, offset, or matched. Optional
regional matching modifies only Geant4 and exports both original and adjusted
simulation values for audit.

Each simulation range is `(start_mm, end_mm, strength, blend_mm)`. A strength of
`0.90` closes 90% of the gap from Geant4 to interpolated measurement inside that
region. Disable `ENABLE_SIMULATION_ADJUSTMENT` for the original comparison.

The PDD panels include a light dashed 100% guide. This indicates the normalized
maximum-dose level; it is not a calculated depth-of-dmax marker. All post-hoc
simulation adjustment must be described transparently and must not be presented
as independent validation.
