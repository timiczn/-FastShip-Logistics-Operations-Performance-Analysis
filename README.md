
### Project Overview

FastShip Logistics operates ten warehouse locations across the United States and partners with seven carriers: UPS, FedEx, DHL, USPS, Amazon Logistics, OnTrac, and LaserShip. The operations manager, Sarah Chen, flagged concerns about rising shipping costs, inconsistent delivery performance, and an uptick in lost and returned packages during 2023.

This project builds the analytical foundation to answer those concerns, covering 19 specific business questions across cost analysis, delivery performance, operational insights, and problem tracking as well as offer recommendations.

<img width="10066" height="5766" alt="Performance Overview" src="https://github.com/user-attachments/assets/c09dd4a7-9425-4d2f-abff-fa7bba1762b9" />


### Dataset

The dataset contains 2,000 shipment records with the following fields:

| Field | Description |
|---|---|
| Shipment_ID | Unique shipment identifier |
| Origin_Warehouse | Warehouse where the shipment originated |
| Destination | Destination city |
| Carrier | Shipping carrier used |
| Shipment_Date | Date the package was shipped |
| Delivery_Date | Date the package was delivered (blank if undelivered) |
| Weight_kg | Package weight in kilograms |
| Cost | Shipping cost in USD |
| Status | Delivery status: Delivered, Delayed, In Transit, Lost, Returned |
| Distance_miles | Distance travelled in miles |
| Transit_Days | Number of days in transit |

### Tools Used

The entirety of this project was built uisng Microsoft Excel

### Data Model

The model consists of two tables connected by date relationships:

**Shipments table:** the main fact table containing all 2,000 shipment records loaded via Power Query directly into the data model.

**Calendar table:** a date dimension table generated using DAX inside Power Pivot, covering the full 2023 date range. Columns include Date, Year, Month Number, Month Name, Month Short, Quarter, and Day of Week.

**Relationships:**
- Active relationship: `Calendar[Date]` to `Shipments[Shipment_Date]` (one-to-many)
- Inactive relationship: `Calendar[Date]` to `Shipments[Delivery_Date]` (one-to-many), activated on demand using `USERELATIONSHIP()` in specific measures

The inactive relationship allows delivery-date-based analysis without disrupting the default shipment-date context across the rest of the model.

### DAX Measures

18 measures were built across the following categories:

**Volume and cost:**
- Total Shipments
- Total Shipping Cost
- Avg Cost Per Shipment
- Total Weight Shipped
- Avg Package Weight
- Cost Per Mile

**Delivery performance:**
- On Time Delivery Rate
- Avg Transit Days
- Deliveries by Delivery Date (uses USERELATIONSHIP)
- Avg Transit by Delivery Date (uses USERELATIONSHIP)

**Status counts:**
- Total Delivered
- Total Delayed
- Total In Transit
- Total Lost
- Total Returned
- Total Lost & Returned

**Problem tracking:**
- Problem Rate
- Avg Cost by Carrier

### Dashboard

The dashboard is built across two pages in Excel using pivot charts, slicers, and linked KPI cards.

**Page 1: Performance Overview**

Covers the executive summary view with five KPI cards (Total Shipments, Total Shipping Cost, On-Time Delivery Rate, Problem Rate, Avg Transit Days), each with a sparkline showing the monthly trend. Charts include: monthly shipment volume and cost trend, shipment status breakdown (donut), top destination cities, top 5 warehouses by deliveries, and monthly delayed shipments trend.

<img width="10066" height="5766" alt="Performance Overview" src="https://github.com/user-attachments/assets/7158ba5a-83a8-4822-b99f-a3dc85677dd3" />


**Page 2: Carrier and Warehouse Analysis**

Covers the deep-dive view with four KPI cards (Most Expensive Carrier, Worst On-Time Carrier, Most Problematic Warehouse, Avg Cost Per Mile). Charts include: carrier performance comparing avg cost per shipment vs on-time delivery rate, lost shipments by carrier, warehouse on-time delivery ranking, and carrier delivery efficiency by avg transit days.

<img width="10066" height="5766" alt="C W Analysis" src="https://github.com/user-attachments/assets/9ab9d6cf-2849-42ad-9c16-c1249e883c62" />


### Key Findings

**Cost:**
- Total shipping spend for 2023 was $401,911.57 at an average of $205.16 per shipment
- UPS is the most expensive carrier at $229.71 per shipment and $0.1756 per mile
- Warehouse LA generated the highest total shipping cost at $49,586.73

**Delivery performance:**
- 82.40% of shipments were delivered on time
- UPS has the best on-time delivery rate at 86.33%
- Amazon Logistics has the worst at 79.20%
- Average transit time across the network is 4.18 days
- Boston has the longest average transit time at 4.50 days

**Operations:**
- Total freight moved was 60,369.60 kg at an average package weight of 30.18 kg
- Chicago was the top destination city with 154 shipments received
- LaserShip handles the most volume at 303 shipments
- Distance and transit time have a strong positive correlation of 0.76

**Problem areas:**
- 276 shipments had problems in 2023, a problem rate of 13.80%
- 45 shipments were lost and 32 were returned, totalling 77 serious failures
- Amazon Logistics and USPS each had 12 lost packages, the most in the network
- Warehouse SF has the highest problem rate at 17.21%
- December had the most problem shipments of any month at 33

### Recommendations

**1. Review the Amazon Logistics partnership.** Amazon Logistics ranks last in on-time delivery rate, has the highest overall problem rate, and ties for the most lost packages. The team should set measurable service level targets and consider redistributing volume to better-performing carriers if improvements are not made.

**2. Audit Warehouse SF and Warehouse HOU.** Both warehouses combine high total shipping costs with the two highest problem rates in the network (17.21% and 16.98%). An operational review of carrier mix, handling procedures, and routing decisions at both locations is a logical starting point.

**3. Optimise carrier selection by route length.** UPS is the most expensive carrier but performs best on long-haul routes where reliability matters most. For shorter routes, USPS offers comparable performance at significantly lower cost ($183 vs $229.71 per shipment). Routing short-distance shipments to USPS more aggressively would reduce overall spend without sacrificing service quality.

**4. Plan ahead for the December spike.** December consistently produced the most problem shipments at 33, well above the monthly average of 23. Increasing carrier diversification in Q4, setting earlier cut-off dates, and adding handling capacity at high-risk warehouses before the holiday period would reduce this predictable seasonal impact.

**5. Use Warehouse CHI as the operational benchmark.** Chicago has the fastest average transit time (3.96 days), the second-lowest problem rate (12.69%), and one of the lowest total shipping costs in the network. Understanding what Chicago does differently and applying those practices to underperforming warehouses is a cost-effective path to improvement.


