MIT 805 - Part 2

Data Processing Using MapReduce
Dataset Description

The original dataset consisted of NYC taxi trip records with multiple attributes such as trip distances, timestamps, pickup and dropoff locations, fares, and surcharges. The file size was approximately 23.6GB containing 140,648,204 records.

MapReduce Workflow
To efficiently process and extract meaningful information from this large dataset, I utilized the MapReduce programming model on Hadoop. The key steps included:
Mapper: Parsed the input data line-by-line, extracting relevant fields such as pickup zone IDs.
Reducer: Aggregated the counts of trips per pickup zone, summing the total trips for each zone.
This distributed approach enabled processing of large datasets by parallelizing computations across nodes, greatly improving scalability and performance compared to single-machine scripts.
Input vs Output Size
Input file size: ~ 23.6 GB
Number of input records: 140,648,204
Output file size: 2,760 bytes
Number of output records: 263
The output was significantly smaller as it summarized counts per pickup zone rather than listing all individual trips.
Technical Interpretation
The MapReduce job completed in approximately ~15 minutes on our Hadoop cluster, demonstrating the power of distributed computing in handling big data tasks.
By aggregating pickup counts, we identified zones with high taxi activity (e.g., zone 107 had over 1 million pickups), which provides a foundation for further analysis such as clustering or demand prediction.

Business Value
Demand Analysis: The counts per zone highlight high-demand areas, allowing taxi services or ride-hailing companies to optimize driver allocation.
Operational Efficiency: Understanding spatial trip distribution can improve dispatching strategies, reducing wait times and increasing customer satisfaction.
Strategic Planning: Data-driven decisions for pricing, promotions, and expanding services in underserved or high-demand areas.
pickupzones.csv: This file contains 
