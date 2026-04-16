# Data Dictionary

Field-level schema for the Uber Rides dataset (50,000 records).

---

| Field | Type | Example | Description |
|-------|------|---------|-------------|
| `trip_id` | INTEGER | 1000421 | Unique identifier for each trip. Primary key. |
| `city` | STRING | "Boston" | Metropolitan area where the trip originated. One of 6 values. |
| `driver_id` | INTEGER | 3847 | Unique identifier for the driver. Not unique per row. |
| `rider_id` | INTEGER | 19203 | Unique identifier for the rider. Not unique per row. |
| `status` | ENUM | "Completed" | Trip outcome: `Completed`, `Cancelled`, or `No-Show`. |
| `fare_amount` | FLOAT | 17.42 | Fare charged in USD. Only meaningful for `Completed` trips. |
| `distance_km` | FLOAT | 6.3 | Trip distance in kilometres. Only populated for `Completed` trips. |
| `timestamp` | DATETIME | 2023-08-14 09:22:11 | UTC timestamp of trip request initiation. |

---

## Status Values

| Value | Meaning | Revenue Impact |
|-------|---------|---------------|
| `Completed` | Rider picked up and dropped off | Full fare collected |
| `Cancelled` | Trip cancelled before pickup (typically rider-initiated) | No fare; possible cancellation fee |
| `No-Show` | Driver arrived; rider absent for >5 minutes | No fare; driver compensation may apply |

---

## City Values

| City | Abbrev. used in charts | Notes |
|------|------------------------|-------|
| Boston | Boston | Highest completion rate in dataset (91%) |
| Chicago | Chicago | Highest fare revenue in dataset |
| Los Angeles | Los Ang. | Longest avg trip distance |
| New York | New York | Largest rider pool |
| San Francisco | San Fran. | Second highest completion rate (89%) |
| Seattle | Seattle | Lowest completion rate (82%) |

---

## Notes on Data Quality

- `fare_amount` is `NULL` for `Cancelled` and `No-Show` records. Exclude these rows when computing average fare.
- `distance_km` follows the same nullability pattern.
- `driver_id` and `rider_id` are persistent across trips — the same driver or rider may appear many times.
- The dataset appears to be **balanced by city** — trip counts per city are within ±5% of each other, which is atypical of real operational data and suggests deliberate sampling.