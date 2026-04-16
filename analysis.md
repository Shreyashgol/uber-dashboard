# Analysis Notes

Analyst commentary, methodology decisions, and open questions.

---

## Methodology

### Fare Average
The $15.98 average fare is computed over **completed trips only**. Including cancelled/no-show records (with NULL fares) would distort the metric. This is the correct approach for any revenue-per-trip KPI.

### Distance Total
350,404 km is similarly restricted to completed trips. A driver who accepts and then cancels a trip has not driven any chargeable distance.

### Rider/Driver Ratio
Computed as `DISTINCT rider_id / DISTINCT driver_id` = 38,352 / 8,963 = 4.28. This is a **pool ratio**, not a per-trip ratio. It measures how many unique riders the driver supply must serve across the full period.

---

## Observations

### Fare Distribution Shape
The histogram shows a classic **right-skewed distribution** with:
- Mode: approximately $17–22 range
- Mean: $15.98 (slightly below mode — consistent with skew)
- Long right tail: fares above $35 represent ~8% of trips but are visually prominent

This shape is consistent with urban ride-hailing markets. Short airport-to-downtown trips dominate the modal bin; occasional long-distance or surge trips extend the tail.

**Recommendation**: Segment fare analysis by trip type (short/medium/long) or time-of-day to understand which segments drive the tail.

---

### City Homogeneity
The similarity of city-level metrics (distance, trips, revenue all within ±15%) is the most analytically suspicious feature of this dataset. Real Uber operational data would show dramatically different volumes — New York alone would likely account for 30–40% of US urban trips.

**Conclusion**: The dataset is likely a **stratified sample** designed for training or demonstration purposes. Conclusions about relative city performance should be treated as illustrative rather than operationally definitive.

---

### Completion Rate Variance
The completion rate range (82% Seattle → 91% Boston) is meaningful even given the sampling caveat:
- An 8-percentage-point spread across markets suggests real operational differences
- Boston's urban density and shorter trip distances likely reduce the friction that causes cancellations
- Seattle's more distributed geography (multiple sub-centers, suburban spread) may create more mismatch between expected and actual pickup points

**Recommended follow-up**: Break completion rate by time-of-day. Pre-work cancellations (8–9am) likely behave differently from late-night no-shows (12–3am).

---

### No-Show vs Cancellation
No-shows (4.4%) are operationally more costly than cancellations (9.4%) despite being less frequent. A cancellation that happens before driver dispatch wastes only rider time; a no-show means a driver has already committed fuel and time.

**Estimated cost of no-shows** at $15.98 avg fare equivalent:
- 2,200 no-show trips × $15.98 opportunity cost = ~$35,156 in foregone revenue
- Plus driver compensation, fuel, and opportunity cost of not taking another trip

---

## Open Questions

1. **Time dimension**: Is there a peak hour or peak day pattern? The timestamp field is present but not yet visualized.
2. **Surge pricing**: Are any fares above the 90th percentile ($38+) attributable to surge? No surge flag in the current dataset.
3. **Driver retention**: Do high-cancellation markets have higher driver churn? Would need a temporal slice to evaluate.
4. **Repeat riders**: What fraction of the 38,352 riders are repeat users? High repeat rate = stickier demand base.
5. **Trip distance by city**: Boston's high completion rate + similar distance to other cities — does Boston actually have shorter individual trips, with more of them completing?

---

## Suggested Next Charts (Roadmap)

| Chart | Question it answers | Priority |
|-------|--------------------|----|
| Hourly trip volume (all cities) | When is demand highest? | High |
| Cancellation rate by hour | Is 8am or midnight worse? | High |
| Fare vs distance scatter | Are long trips underpriced? | Medium |
| Rider cohort retention | How sticky is the rider base? | Medium |
| Driver utilisation rate | How many trips per active hour? | Low |