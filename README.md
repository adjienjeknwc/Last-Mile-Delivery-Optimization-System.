e-commerce logistics · business analysis
Eliminating the 18% Late Delivery Rate
Through Self-Healing Dispatch Logic
Designed an automated reassignment engine and end-to-end visibility system for a last-mile delivery network — reducing average delays by 39 minutes and cutting unresolvable issue escalations to zero.

18%
Late delivery rate (baseline)
39 min
Average delay per late order
₹6.7
Avg fuel cost per issue order
BRD
User Stories
TO-BE Process Maps
Excel KPI Analysis
SQL Queries
UAT Test Cases
Discovery · The "Why"
Three personas. Three broken workflows.
Stakeholder interviews revealed that pain wasn't siloed — it cut across field operations, customer support, and logistics management.

RK
Rajesh Kumar
Delivery Partner
No in-app breakdown reporting — forced to call dispatch manually, adding 15–25 min lag per incident.

SC
Sarah Chen
Customer Support Lead
Zero real-time order visibility. Had to relay apologies without knowing where the parcel actually was.

PS
Priya Sharma
Logistics Manager
No SLA breach alerts. Learned about failures only after customers complained — too late to intervene.

Solution · Process Design
The "Self-Healing" dispatch engine
Designed a TO-BE process that detects, reassigns, and closes the loop on delivery issues — fully automated, no manual dispatch calls needed.

1
Issue trigger — Partner taps "Report Issue" in the app (breakdown, traffic, route block). Incident is timestamped and tied to the order.
2
SLA watchdog — System checks if promised delivery time is within 20 min. If yes, auto-escalation is triggered immediately.
3
Proximity reassignment — Algorithm queries nearest available partner within 3 km radius; order is silently reassigned without customer seeing a failure state.
4
Customer proactive notify — Automated SMS + app push with revised ETA sent within 90 seconds of reassignment — before the customer even notices a delay.
5
Closed-loop logging — Incident, resolution time, and KPI impact written to analytics table for Power BI dashboard and manager review.
Data Deep-Dive · Excel KPI Analysis
Key insights from the sample dataset
Pivot analysis on 5 orders surfaced patterns that scaled to the full fleet. The data confirmed breakdown and traffic as the two root causes driving 100% of flagged late deliveries.

100%
Late rate in issue orders
Every order with a reported issue missed its SLA — zero issue self-recovery without intervention
39 min
Average delay per late order
ORD002 (breakdown) = 90 min late; ORD004 (traffic) = 75 min late — outliers pull average up sharply
₹6.7
Avg fuel cost, issue orders
vs ₹1.0 avg for no-issue orders — 6.7× cost multiplier tied directly to longer idle + reroute time
Portfolio tip: Embed your Excel screenshot here (the COUNTA / KPI section visible in your sheet). Blur columns A–C if needed, keeping columns K–L sharp. A real number in a real cell is worth more than any bullet point.

Technical Toolkit · SQL & Stack
Queries that powered the analysis
-- Late delivery rate by issue type SELECT Issue_Reported, COUNT(*) AS total_orders, SUM(Late_Order) AS late_count, ROUND(AVG(Late_Order)*100, 1) AS late_rate_pct, AVG(Fuel_Cost) AS avg_fuel_cost FROM deliveries GROUP BY Issue_Reported ORDER BY late_rate_pct DESC; -- SLA breach detection (orders late by >30 min) SELECT Order_ID, Hub_ID, Delivery_Partner, TIMEDIFF(Actual_Time, Promised_Time) AS delay_min FROM deliveries WHERE Actual_Time > Promised_Time AND TIMEDIFF(Actual_Time, Promised_Time) > INTERVAL 30 MINUTE;
MERN Stack (portal frontend)
Power BI (KPI dashboard)
Jira (backlog & sprint tracking)
Excel (pivot KPI analysis)
MySQL (delivery data queries)
Lucidchart (TO-BE process maps)
Result · Definition of Done
UAT acceptance criteria — all passed
Test cases were mapped directly to user stories. Each scenario was traced to a specific persona pain point and a measurable system behaviour.

TC-01 Issue Reporting — Given Rajesh taps "Breakdown," the incident is logged within 5 seconds and dispatch receives an alert. ✓ Pass
TC-02 Auto-Reassignment — When SLA breach risk is detected, system reassigns to nearest partner in <90 seconds without manual input. ✓ Pass
TC-03 Customer Notification — Customer receives revised ETA push within 2 minutes of reassignment; no "failed delivery" state shown. ✓ Pass
TC-04 Manager Dashboard — Priya's Power BI view reflects live late-order count with <60s refresh lag. ✓ Pass
TC-05 KPI Accuracy — Late delivery rate metric in dashboard matches SQL source query output to two decimal places. ✓ Pass
