# NPD TRACKING DASHBOARD - TEST CASES

## 📋 OVERVIEW

This document provides comprehensive test scenarios to validate the NPD Tracking Dashboard functionality and ensure governance compliance.

---

## 🎯 TESTING OBJECTIVES

1. Verify data loading and parsing
2. Validate risk calculation logic
3. Test filter functionality
4. Confirm responsive design
5. Ensure governance compliance

---

## ✅ TEST CASE CATEGORIES

### **Category A: Data Loading**
### **Category B: Risk Calculation**
### **Category C: Fragrance Board**
### **Category D: ETA Tracker**
### **Category E: Filters**
### **Category F: Timeline**
### **Category G: Responsive Design**
### **Category H: Governance Compliance**

---

## 📊 CATEGORY A: DATA LOADING

### **Test A1: Auto-Load Excel File**

**Objective:** Verify dashboard automatically loads NPD_REPORT.xlsx

**Steps:**
1. Place NPD_REPORT.xlsx in same folder as HTML
2. Open NPD-Tracking-Dashboard.html in browser
3. Wait 2 seconds

**Expected Result:**
- ✅ Dashboard populates with data from Excel
- ✅ KPI cards show counts
- ✅ Fragrance Board displays projects
- ✅ No "No data available" message

**Status:** [ ] Pass [ ] Fail

**Notes:**
_________________________________

---

### **Test A2: Manual File Upload**

**Objective:** Verify drag-and-drop upload works

**Steps:**
1. Open dashboard without NPD_REPORT.xlsx in folder
2. Drag Excel file onto upload area
3. Drop file

**Expected Result:**
- ✅ Upload area highlights on drag
- ✅ Data loads immediately after drop
- ✅ Dashboard populates correctly

**Status:** [ ] Pass [ ] Fail

**Notes:**
_________________________________

---

### **Test A3: Invalid File Handling**

**Objective:** Verify error handling for non-Excel files

**Steps:**
1. Try to upload a .txt or .pdf file
2. Observe behavior

**Expected Result:**
- ✅ File input only accepts .xlsx/.xls
- ✅ Invalid files rejected gracefully
- ✅ No error crash

**Status:** [ ] Pass [ ] Fail

**Notes:**
_________________________________

---

## 🎲 CATEGORY B: RISK CALCULATION

### **Test B1: High Risk - Missing Materials**

**Test Data:**
```
DCR: TEST-001
Launch Date: 25 days from today
Bottle Status: Not Ordered
Box Status: Received
Oil Status: Ordered
```

**Expected Result:**
- ✅ Risk Level: HIGH
- ✅ Risk Label: "AT RISK"
- ✅ Badge color: Red
- ✅ Appears in High Risk tab

**Reason:** Launch ≤ 30 days AND critical material missing

**Status:** [ ] Pass [ ] Fail

**Notes:**
_________________________________

---

### **Test B2: On Track Override - All Received**

**Test Data:**
```
DCR: TEST-002
Launch Date: 10 days from today
Bottle Status: Received
Box Status: Received
Oil Status: Received
```

**Expected Result:**
- ✅ Risk Level: LOW
- ✅ Risk Label: "ON TRACK"
- ✅ Badge color: Green
- ✅ Appears in On Track tab

**Reason:** All materials received (override rule)

**Status:** [ ] Pass [ ] Fail

**Notes:**
_________________________________

---

### **Test B3: Medium Risk - Watch Window**

**Test Data:**
```
DCR: TEST-003
Launch Date: 45 days from today
Bottle Status: Not Ordered
Box Status: Ordered
Oil Status: Ordered
```

**Expected Result:**
- ✅ Risk Level: MEDIUM
- ✅ Risk Label: "WATCH"
- ✅ Badge color: Yellow/Orange
- ✅ Appears in Medium Risk tab

**Reason:** Launch 31-60 days AND material missing

**Status:** [ ] Pass [ ] Fail

**Notes:**
_________________________________

---

### **Test B4: On Track - Adequate Lead Time**

**Test Data:**
```
DCR: TEST-004
Launch Date: 75 days from today
Bottle Status: Ordered
Box Status: Ordered
Oil Status: Received
```

**Expected Result:**
- ✅ Risk Level: LOW
- ✅ Risk Label: "ON TRACK"
- ✅ Badge color: Green
- ✅ Appears in On Track tab

**Reason:** Launch 61-90 days AND all materials ordered/received

**Status:** [ ] Pass [ ] Fail

**Notes:**
_________________________________

---

### **Test B5: No Launch Date**

**Test Data:**
```
DCR: TEST-005
Launch Date: (blank)
Bottle Status: Not Ordered
Box Status: Not Ordered
Oil Status: Not Ordered
```

**Expected Result:**
- ✅ Risk Level: MEDIUM
- ✅ Risk Label: "WATCH"
- ✅ Does not crash
- ✅ Default risk applied

**Status:** [ ] Pass [ ] Fail

**Notes:**
_________________________________

---

## 📋 CATEGORY C: FRAGRANCE BOARD

### **Test C1: Pending Column**

**Test Data:**
```
6 projects with Fragrance Status = "Pending"
```

**Expected Result:**
- ✅ Pending column shows 6 cards
- ✅ Count badge shows "6"
- ✅ Each card displays DCR, Name, Brand
- ✅ Cards have orange left border
- ✅ Subtitle: "Fragrance not identified"

**Status:** [ ] Pass [ ] Fail

**Notes:**
_________________________________

---

### **Test C2: Direction Set Column**

**Test Data:**
```
4 projects with Fragrance Status = "Direction Set"
```

**Expected Result:**
- ✅ Direction Set column shows 4 cards
- ✅ Count badge shows "4"
- ✅ Cards have yellow/orange left border
- ✅ Subtitle: "Benchmark / olfactive direction locked"

**Status:** [ ] Pass [ ] Fail

**Notes:**
_________________________________

---

### **Test C3: Finalized Column**

**Test Data:**
```
9 projects with Fragrance Status = "Finalized"
```

**Expected Result:**
- ✅ Finalized column shows 9 cards
- ✅ Count badge shows "9"
- ✅ Cards have teal left border
- ✅ Subtitle: "Fragrance approved"

**Status:** [ ] Pass [ ] Fail

**Notes:**
_________________________________

---

### **Test C4: Card Hover Effect**

**Steps:**
1. Hover over any fragrance board card
2. Observe animation

**Expected Result:**
- ✅ Card slides right slightly
- ✅ Shadow appears
- ✅ Smooth transition
- ✅ Cursor remains default (no pointer)

**Status:** [ ] Pass [ ] Fail

**Notes:**
_________________________________

---

## 📊 CATEGORY D: ETA TRACKER

### **Test D1: Risk Tab Filtering**

**Steps:**
1. Click "High Risk" tab
2. Verify table shows only high-risk projects
3. Click "Medium Risk" tab
4. Verify table shows only medium-risk projects
5. Click "On Track" tab
6. Verify table shows only on-track projects

**Expected Result:**
- ✅ Active tab has solid background color
- ✅ Table filters correctly
- ✅ Counts in tabs are accurate
- ✅ Tab switching is smooth

**Status:** [ ] Pass [ ] Fail

**Notes:**
_________________________________

---

### **Test D2: Material Status Display**

**Test Data:**
```
Project 1: Bottle = "Not Ordered"
Project 2: Bottle = "Ordered", ETA = March 15
Project 3: Bottle = "Received"
```

**Expected Result:**
- ✅ Project 1: Red badge "❌ Not Ordered"
- ✅ Project 2: Yellow badge "🟡 MAR 15"
- ✅ Project 3: Green badge "✅ Received"

**Status:** [ ] Pass [ ] Fail

**Notes:**
_________________________________

---

### **Test D3: Table Responsiveness**

**Steps:**
1. Resize browser to mobile width (< 480px)
2. Scroll table horizontally
3. Verify all columns accessible

**Expected Result:**
- ✅ Table scrolls horizontally
- ✅ All columns visible with scroll
- ✅ Header stays readable
- ✅ Text doesn't wrap awkwardly

**Status:** [ ] Pass [ ] Fail

**Notes:**
_________________________________

---

## 🔍 CATEGORY E: FILTERS

### **Test E1: Brand Filter**

**Steps:**
1. Click Brand filter in sidebar
2. Select "ARMAF"
3. Verify dashboard updates

**Expected Result:**
- ✅ Only ARMAF projects shown
- ✅ KPIs update to ARMAF counts
- ✅ Fragrance Board shows only ARMAF
- ✅ ETA Tracker filtered
- ✅ Timeline filtered

**Status:** [ ] Pass [ ] Fail

**Notes:**
_________________________________

---

### **Test E2: Multiple Brand Selection**

**Steps:**
1. Select "ARMAF" and "CDN"
2. Verify both brands shown

**Expected Result:**
- ✅ Projects from both brands visible
- ✅ Other brands excluded
- ✅ Counts accurate

**Status:** [ ] Pass [ ] Fail

**Notes:**
_________________________________

---

### **Test E3: Status Filter**

**Steps:**
1. Select "Designing" status
2. Verify only designing projects shown

**Expected Result:**
- ✅ Filter applied across all sections
- ✅ Designing KPI matches filtered count
- ✅ Other statuses hidden

**Status:** [ ] Pass [ ] Fail

**Notes:**
_________________________________

---

### **Test E4: Search Function**

**Steps:**
1. Type "DCR-2025-2036" in search
2. Verify results

**Expected Result:**
- ✅ Only matching DCR shown
- ✅ Real-time filtering
- ✅ Case-insensitive search
- ✅ Clear search works

**Status:** [ ] Pass [ ] Fail

**Notes:**
_________________________________

---

### **Test E5: Combined Filters**

**Steps:**
1. Select Brand = "ARMAF"
2. Select Status = "Designing"
3. Type "Yum" in search

**Expected Result:**
- ✅ All filters applied together
- ✅ Only projects matching ALL criteria shown
- ✅ Dashboard remains responsive

**Status:** [ ] Pass [ ] Fail

**Notes:**
_________________________________

---

## 📅 CATEGORY F: TIMELINE

### **Test F1: Timeline Filter - All**

**Steps:**
1. Select "All" timeline filter
2. Verify all projects shown on timeline

**Expected Result:**
- ✅ All projects with launch dates visible
- ✅ Bars positioned by date
- ✅ Color-coded by risk

**Status:** [ ] Pass [ ] Fail

**Notes:**
_________________________________

---

### **Test F2: Timeline Filter - This Month**

**Steps:**
1. Select "This Month" filter
2. Verify only current month launches shown

**Expected Result:**
- ✅ Only projects launching this month visible
- ✅ Other months excluded
- ✅ Filter updates timeline immediately

**Status:** [ ] Pass [ ] Fail

**Notes:**
_________________________________

---

### **Test F3: Timeline Filter - Next 30 Days**

**Steps:**
1. Select "Next 30 Days" filter
2. Verify upcoming launches shown

**Expected Result:**
- ✅ Only projects launching within 30 days
- ✅ Past launches excluded
- ✅ Far future launches excluded

**Status:** [ ] Pass [ ] Fail

**Notes:**
_________________________________

---

### **Test F4: Timeline Filter - High Risk Only**

**Steps:**
1. Select "High Risk Only" filter
2. Verify only high-risk projects shown

**Expected Result:**
- ✅ Only red (high risk) bars visible
- ✅ Medium and low risk excluded
- ✅ Consistent with ETA Tracker high risk

**Status:** [ ] Pass [ ] Fail

**Notes:**
_________________________________

---

### **Test F5: Timeline Bar Hover**

**Steps:**
1. Hover over a timeline bar
2. Observe tooltip

**Expected Result:**
- ✅ Tooltip shows project name and date
- ✅ Bar lifts slightly
- ✅ Shadow appears
- ✅ Cursor shows pointer

**Status:** [ ] Pass [ ] Fail

**Notes:**
_________________________________

---

## 📱 CATEGORY G: RESPONSIVE DESIGN

### **Test G1: Desktop View (1920x1080)**

**Expected Result:**
- ✅ Sidebar: 120px width
- ✅ KPI cards: 4 columns
- ✅ Fragrance Board: 3 columns
- ✅ All elements properly spaced
- ✅ No horizontal scroll

**Status:** [ ] Pass [ ] Fail

**Notes:**
_________________________________

---

### **Test G2: Tablet View (768px)**

**Steps:**
1. Resize to 768px width
2. Verify layout adapts

**Expected Result:**
- ✅ Sidebar: 80px width
- ✅ KPI cards: 2x2 grid
- ✅ Fragrance Board: Stacked vertically
- ✅ Readable text sizes
- ✅ Touch-friendly spacing

**Status:** [ ] Pass [ ] Fail

**Notes:**
_________________________________

---

### **Test G3: Mobile View (375px)**

**Steps:**
1. Resize to 375px width (iPhone size)
2. Test all interactions

**Expected Result:**
- ✅ All sections stack vertically
- ✅ Tables scroll horizontally
- ✅ Risk tabs stack or scroll
- ✅ Filters accessible
- ✅ No layout breaks

**Status:** [ ] Pass [ ] Fail

**Notes:**
_________________________________

---

### **Test G4: Mobile Sidebar**

**Steps:**
1. On mobile, tap sidebar icons
2. Verify dropdowns work

**Expected Result:**
- ✅ Dropdowns appear as bottom sheets
- ✅ Easy to select options
- ✅ Close button works
- ✅ Overlay darkens background

**Status:** [ ] Pass [ ] Fail

**Notes:**
_________________________________

---

## ✅ CATEGORY H: GOVERNANCE COMPLIANCE

### **Test H1: Locked Fragrance Stages**

**Objective:** Verify only 3 fragrance stages exist

**Steps:**
1. Review Excel data
2. Verify all Fragrance Status values

**Expected Result:**
- ✅ Only "Pending", "Direction Set", "Finalized" used
- ✅ No other values accepted
- ✅ Misspellings cause items to not appear

**Status:** [ ] Pass [ ] Fail

**Notes:**
_________________________________

---

### **Test H2: Material Status Validation**

**Objective:** Verify only 3 material statuses accepted

**Steps:**
1. Check Bottle, Box, Oil Status columns
2. Verify all use approved values

**Expected Result:**
- ✅ Only "Not Ordered", "Ordered", "Received"
- ✅ No free text
- ✅ No subjective descriptions

**Status:** [ ] Pass [ ] Fail

**Notes:**
_________________________________

---

### **Test H3: System-Driven Risk**

**Objective:** Confirm risk cannot be manually overridden

**Steps:**
1. Look for any "Risk" column in Excel
2. Verify dashboard calculates risk

**Expected Result:**
- ✅ No Risk column in Excel
- ✅ Dashboard calculates risk automatically
- ✅ Risk based solely on dates and materials
- ✅ Consistent logic applied

**Status:** [ ] Pass [ ] Fail

**Notes:**
_________________________________

---

### **Test H4: Read-Only Dashboard**

**Objective:** Confirm dashboard doesn't allow editing

**Steps:**
1. Try to click/edit any data in dashboard
2. Verify no edit capability

**Expected Result:**
- ✅ No input fields in dashboard
- ✅ No save/update buttons
- ✅ Read-only viewing only
- ✅ Changes must be in Excel

**Status:** [ ] Pass [ ] Fail

**Notes:**
_________________________________

---

### **Test H5: ETA Display Logic**

**Objective:** Verify ETAs only shown when "Ordered"

**Test Data:**
```
Material Status: "Not Ordered", ETA: (any date)
```

**Expected Result:**
- ✅ Shows "❌ Not Ordered" regardless of ETA value
- ✅ ETA ignored if status is not "Ordered"

**Test Data:**
```
Material Status: "Received", ETA: (any date)
```

**Expected Result:**
- ✅ Shows "✅ Received"
- ✅ No date displayed

**Test Data:**
```
Material Status: "Ordered", ETA: March 15
```

**Expected Result:**
- ✅ Shows "🟡 MAR 15"
- ✅ Date formatted correctly

**Status:** [ ] Pass [ ] Fail

**Notes:**
_________________________________

---

## 📊 TEST SUMMARY TEMPLATE

### **Testing Session Information**

**Tester Name:** _______________________
**Date:** _______________________
**Browser:** _______________________
**Browser Version:** _______________________
**Operating System:** _______________________
**Screen Resolution:** _______________________

### **Overall Results**

| Category | Tests Passed | Tests Failed | Pass Rate |
|----------|--------------|--------------|-----------|
| A: Data Loading | ___/3 | ___ | ___% |
| B: Risk Calculation | ___/5 | ___ | ___% |
| C: Fragrance Board | ___/4 | ___ | ___% |
| D: ETA Tracker | ___/3 | ___ | ___% |
| E: Filters | ___/5 | ___ | ___% |
| F: Timeline | ___/5 | ___ | ___% |
| G: Responsive Design | ___/4 | ___ | ___% |
| H: Governance Compliance | ___/5 | ___ | ___% |
| **TOTAL** | **___/34** | **___** | **___%** |

### **Critical Issues Found**

1. _________________________________
2. _________________________________
3. _________________________________

### **Minor Issues Found**

1. _________________________________
2. _________________________________
3. _________________________________

### **Recommendations**

1. _________________________________
2. _________________________________
3. _________________________________

### **Sign-Off**

**Tester Signature:** _______________________

**Date:** _______________________

**Approved for Production:** [ ] Yes [ ] No

**Approver Name:** _______________________

**Approver Signature:** _______________________

---

## 🎯 ACCEPTANCE CRITERIA

**Dashboard is ready for production when:**

- ✅ ALL Category A (Data Loading) tests pass
- ✅ ALL Category B (Risk Calculation) tests pass
- ✅ ALL Category H (Governance Compliance) tests pass
- ✅ At least 90% of other tests pass
- ✅ No critical issues remain
- ✅ Performance is acceptable (<2 second load time)
- ✅ Works on Chrome, Edge, Firefox, Safari
- ✅ Mobile-responsive (tested on actual devices)

---

## 📋 REGRESSION TEST SCHEDULE

**When to Re-Test:**
- After any code changes
- After Excel template updates
- Monthly as part of maintenance
- After browser updates
- When new features added

**Quick Smoke Test (15 minutes):**
- Test A1 (Auto-load)
- Test B1, B2 (High risk, On track override)
- Test C1, C2, C3 (All fragrance columns)
- Test D1 (Risk tabs)
- Test H3 (System risk)

**Full Regression Test (2 hours):**
- All test cases in this document
- Additional edge cases discovered
- Performance testing
- Cross-browser verification

---

END OF TEST CASES DOCUMENT
