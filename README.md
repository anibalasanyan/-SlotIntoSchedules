# SlotIntoSchedules

# ✈️ Slot Integration into Flight Schedules

This module integrates **airport slot information** into flight schedule planning, supporting both **operational tracking** and **visual slot allocation** in planning tools like Route View.

Airports with strict slot control (e.g., JFK, LHR) allocate departure and arrival windows. These slots must be matched to scheduled flights in order to ensure airspace coordination, reduce delays, and meet regulatory requirements.

---

## 🎯 Purpose

- Visually associate real-world slot approvals with internal flight schedules
- Highlight mismatches or missing slots early in the planning process
- Track status of slot negotiations (Confirmed, Requested, Denied)
- Provide planners with actionable feedback through integrated UI

---

## ⚙️ Core Logic

### 🧩 Flow Overview

AttachSlotsInteractor
├── closuresToClean → Removes outdated slot data
├── ConstructDepArrSlotTasks → Splits raw slot data into dep/arr tasks
└── BuildResult → Finds matching slots for each flight/day


### 🔍 Slot Matching Criteria

A **departure slot** is considered matched when:

- `IsDeparture()` returns true  
- Slot’s operation date includes the flight date  
- Slot’s airport = flight origin  
- `DepartureTo` = flight destination  
- Airline code and flight number match  
- Optional `OpsSuffix` matches (or is null and blank)

**Arrival slots** use the same structure but reversed (`ArrivalFrom`, `ArrivalFlightNumber`, etc.)

---

## ✅ Slot Match Logic

The system selects the most appropriate slot by:

- Choosing the **confirmed slot** with status `Accepted` (if available)
- Otherwise, choosing a **requested slot** with `MessageStatus == Requested`

If no valid slot is found, the flight is marked as **unslotted** for that day.

---

## 🧾 Slot Metadata Captured in Closures

Matched slot data is stored in session closures for downstream usage and display in the UI:

| Field             | Description                                      |
|------------------|--------------------------------------------------|
| `SlotDeptStatus` | None, Confirmed, Denied, Requested               |
| `SlotDeptTime`   | Assigned time for departure                      |
| `SlotDeptOffer`  | Offered departure time                           |
| `SlotDeptReq`    | Requested time                                   |
| `SlotDeptWL`     | Waitlist time                                    |
| `SlotArvlStatus` | Same for arrival                                 |
| `SlotArvlTime`   | Assigned time for arrival                        |
| `DeptSlotInfo`   | Combined display, e.g., "Confirmed at 14:10"     |
| `ArvlSlotInfo`   | Same for arrival                                 |

---

## 🖥️ Integration with Route View

Slot values are rendered alongside each flight leg in the Route View:

- Color-coded status indicators (green = confirmed, yellow = requested)
- Tooltip shows full timing info on hover
- Helps identify gaps, conflicts, or pending approvals visually

---

## 💼 Business Value

- Reduces airport slot rejections through early validation
- Improves day-of-operations readiness and reliability
- Supports compliance with airport authorities and coordination bodies
- Enhances planner experience with live visual context

---

## 👩‍💻 Author

**Ani Balasanyan**  
Senior Software Engineer — Backend & Distributed Systems  
[GitHub](https://github.com/anibalasanyan) | [LinkedIn](https://linkedin.com/in/anibalasanyan)

