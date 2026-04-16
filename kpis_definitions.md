# KPI Definitions

This document defines every metric shown on the Uber Rides Analytics Dashboard.

---

## Primary KPIs

### Cities
**Definition**: Count of distinct metropolitan areas in the dataset.  
**Value**: 6  
**Markets**: Boston, Chicago, Los Angeles, New York, San Francisco, Seattle  
**Why it matters**: Market count determines operational breadth and benchmarking baseline.

---

### Total Trips
**Definition**: Count of all trip records regardless of status (Completed, Cancelled, No-Show).  
**Value**: 50,000  
**Formula**: `COUNT(trip_id)`  
**Why it matters**: Top-line volume metric. Includes all lifecycle states so it represents true demand signal, not just revenue-generating events.

---

### Unique Drivers
**Definition**: Count of distinct `driver_id` values in the dataset.  
**Value**: 8,963  
**Formula**: `COUNT(DISTINCT driver_id)`  
**Derived metric**: Trips-per-driver = 50,000 / 8,963 = **5.58 trips/driver**  
**Why it matters**: Supply-side health indicator. A declining driver count relative to trip volume signals supply pressure and potential surge risk.

---

### Unique Riders
**Definition**: Count of distinct `rider_id` values in the dataset.  
**Value**: 38,352  
**Formula**: `COUNT(DISTINCT rider_id)`  
**Derived metric**: Rider-to-driver ratio = 38,352 / 8,963 = **4.28 : 1**  
**Why it matters**: Demand-side health. A high ratio indicates strong rider base relative to driver supply — good for driver earnings, potential upward fare pressure.

---

### Total Distance (km)
**Definition**: Sum of `distance_km` across all trips where status = 'Completed'.  
**Value**: 350,404 km  
**Formula**: `SUM(distance_km) WHERE status = 'Completed'`  
**Why it matters**: Operational scale metric. Correlates with fuel costs, vehicle wear, and driver payout basis in many markets.

---

### Average Fare Amount
**Definition**: Mean fare across all completed trips.  
**Value**: $15.98  
**Formula**: `AVG(fare_amount) WHERE status = 'Completed'`  
**Why it matters**: Revenue-per-trip benchmark. Should be monitored alongside the fare distribution — a rising mean that's driven by long-tail surge pricing tells a different story than a rising mean from base fare growth.

---

## Secondary Metrics (Charts)

### Completion Rate
**Definition**: Percentage of trips with status = 'Completed'.  
**Value**: 86.2%  
**Formula**: `COUNT(status='Completed') / COUNT(*) * 100`  
**Healthy range**: 80–90% for urban markets  
**Red flags**: Below 75% may indicate driver reliability issues; above 95% may suggest status coding errors.

---

### Cancellation Rate
**Definition**: Percentage of trips with status = 'Cancelled'.  
**Value**: 9.4%  
**Note**: Cancellations are primarily rider-initiated. Driver-initiated cancellations should be tracked separately if available.

---

### No-Show Rate
**Definition**: Percentage of trips where driver arrived but rider was absent.  
**Value**: 4.4%  
**Note**: Higher operational cost than cancellation — driver has already committed time and fuel.

---

### Fare Revenue by City
**Definition**: Sum of `fare_amount` for completed trips, grouped by `city`.  
**Note**: Presented in $K (thousands). Used to rank cities by absolute revenue contribution.

---

### Trip Volume by City
**Definition**: Count of trips (all statuses), grouped by `city`, stacked by status.  
**Note**: Stacked presentation allows simultaneous comparison of total volume and status mix.

---

### Distance by City
**Definition**: Sum of `distance_km` for completed trips, grouped by `city`.  
**Note**: Presented in K (thousands). Comparable distances across cities suggest similar market geographies or dataset balancing.