# 010: Check what the experts' AFM height measures

- **Status:** active
- **Created:** 2026-11-22
- **Updated:** 2026-11-22
- **Due:** 2026-12-09

## Goal

A B4 cell in `project11.ipynb`, after B3, tests the assumption that the experts' `afm_height` is the maximum height above the substrate. It matches each expert silica particle to a segmented particle by centroid on the same map. It reports how many match, the distance to the map edge of any that do not, and the median and range of my height − expert height, in one figure followed by a sentence giving the numbers. The task is done when that difference has been compared with the substrate noise σ, and the result is recorded here and in the decisions log.

## Progress log

### 2026-11-22

Created for after the oral check-in on 2026-11-24. The draft only names this assumption and its check in Next steps. The expert `afm_height` is defined in none of the sources checked (decisions-log D2; task 004). Planned check: centroid matching, which needs a centroid column added to the B2 cell and a `MATCH_RADIUS_PX` parameter.

## Outcome
