# NPD TRACKING DASHBOARD - COMPLETE PACKAGE

## 🎉 OVERVIEW

This is your **complete NPD Tracking Dashboard** built according to the governance model specifications. Everything you need is included in this package.

---

## 📦 WHAT'S INCLUDED

### **1. NPD-Tracking-Dashboard.html**
The complete dashboard application - a single, standalone HTML file containing:
- Auto-load functionality for Excel data
- Fragrance Board with 3 locked stages
- Risk-based ETA Tracker
- Interactive timeline with filters
- Responsive design for all devices
- All features specified in the governance model

### **2. NPD_REPORT.xlsx**
Ready-to-use Excel file with:
- All required columns properly formatted
- 18 sample projects demonstrating all scenarios
- Test cases for risk calculation
- Examples of all fragrance stages
- Material status demonstrations

### **3. SAMPLE-DATA.xlsx**
Duplicate of NPD_REPORT.xlsx for reference/backup

### **4. SETUP-GUIDE.md**
Comprehensive deployment guide covering:
- SharePoint deployment steps
- Local network deployment
- Excel file requirements
- Data update workflow
- User guide for all features
- Troubleshooting section
- Mobile access instructions

### **5. TEST-CASES.md**
Complete testing documentation with:
- 34 detailed test scenarios
- Risk calculation validation
- Governance compliance checks
- Responsive design testing
- Acceptance criteria
- Testing summary templates

---

## 🚀 QUICK START (5 MINUTES)

### **Option A: Local Testing**

1. **Download all files** to the same folder on your computer

2. **Double-click** `NPD-Tracking-Dashboard.html`
   - Opens in your default browser
   - Automatically loads data from `NPD_REPORT.xlsx`
   - Sample data displays immediately

3. **Explore the dashboard**:
   - View KPI cards at the top
   - See Fragrance Board with projects in 3 stages
   - Click risk tabs in ETA Tracker
   - Use sidebar filters
   - Try timeline filters

### **Option B: SharePoint Deployment**

1. **Upload both files** to SharePoint:
   - `NPD-Tracking-Dashboard.html`
   - `NPD_REPORT.xlsx`
   - Must be in the **same folder**

2. **Click the HTML file** in SharePoint
   - Select "Open in Browser"
   - Data loads automatically
   - Bookmark the URL

3. **Share the link** with your team

---

## 🎯 KEY FEATURES

### **Fragrance Board** (NEW)
Visual Kanban-style board showing progression through:
- **Pending**: Fragrance not identified (Orange)
- **Direction Set**: Benchmark/direction locked (Yellow/Orange)
- **Finalized**: Fragrance approved (Teal)

Each column shows count and project cards with DCR, Name, and Brand.

### **Risk-Based ETA Tracker** (NEW)
Filter projects by risk level:
- **High Risk** (Red): Launch ≤30 days with materials missing
- **Medium Risk** (Yellow): Launch 31-60 days with materials missing
- **On Track** (Green): All materials received OR adequate lead time

**Risk is calculated automatically** - not manually set!

### **Material Status Display**
Clear visual indicators:
- ❌ Red: Not Ordered
- 🟡 Yellow with date: Ordered (shows ETA)
- ✅ Green: Received

### **Interactive Filters**
Sidebar filters for:
- Brand (multi-select)
- Status (multi-select)
- Search (DCR, Brand, Name)
- Launch Month

### **Timeline Visualization**
Gantt-style timeline with filters:
- All projects
- This month only
- Next 30 days
- High risk only

Color-coded bars show risk levels at a glance.

---

## 📊 GOVERNANCE MODEL COMPLIANCE

This dashboard implements the governance principles from your PDF:

✅ **Locked Fragrance Stages**
- Only 3 stages allowed
- No skipping stages
- Clear progression path

✅ **Fact-Based Material Status**
- Only 3 values: Not Ordered / Ordered / Received
- No subjective text
- ETAs only when Ordered

✅ **System-Driven Risk**
- Calculated automatically
- Based on launch date + material readiness
- Cannot be manually overridden
- Consistent logic applied to all projects

✅ **Single Source of Truth**
- All data in one Excel file
- Dashboard is read-only
- Updates happen in Excel only
- No direct editing in dashboard

---

## 🎨 HOW IT WORKS

### **Data Flow**

```
NPD_REPORT.xlsx (Excel)
         ↓
  [Auto-Load on Page Load]
         ↓
  Parse & Validate Data
         ↓
  Calculate Risk Levels
         ↓
  Render Dashboard Components
         ↓
  User Interacts (Filter/Search)
         ↓
  Update Display in Real-Time
```

### **Risk Calculation Logic**

The system automatically calculates risk based on:

**INPUT:**
- Days until launch date
- Bottle status (Not Ordered / Ordered / Received)
- Box status (Not Ordered / Ordered / Received)  
- Oil status (Not Ordered / Ordered / Received)

**RULES:**
1. **ON TRACK OVERRIDE**: All materials received → Always ON TRACK
2. **HIGH RISK**: Launch ≤30 days AND any material missing → AT RISK
3. **MEDIUM RISK**: Launch 31-60 days AND any material missing → WATCH
4. **ON TRACK**: Launch 61-90 days AND all ordered/received → ON TRACK

No manual intervention needed - the system does it all!

---

## 📝 USING YOUR OWN DATA

### **Step 1: Update Excel File**

Open `NPD_REPORT.xlsx` and:

1. **Keep the header row** (row 1) exactly as is
2. **Delete sample data** (rows 2 onwards)
3. **Add your projects** starting from row 2

### **Step 2: Follow Column Rules**

**Required Columns:**

- **DCR**: Project identifier (e.g., DCR-2025-2036)
- **Brand**: ARMAF, CDN, FLAVIA, ODC, etc.
- **Final Name**: Product name
- **Fragrance Status**: ONLY use:
  - `Pending`
  - `Direction Set`
  - `Finalized`
- **Overall Status**: ONLY use:
  - `Designing`
  - `Sampling`
  - `Approval`
  - `Approved`
- **Launch Date**: Excel date (MM/DD/YYYY)
- **Bottle Status**: ONLY use:
  - `Not Ordered`
  - `Ordered`
  - `Received`
- **Bottle ETA**: Date (only if status is "Ordered")
- **Box Status**: Same options as Bottle
- **Box ETA**: Date (only if status is "Ordered")
- **Oil Status**: Same options as Bottle
- **Oil ETA**: Date (only if status is "Ordered")

### **Step 3: Save & Refresh**

1. **Save** the Excel file
2. **Refresh** your browser (F5 or Ctrl+R)
3. Dashboard updates with new data instantly

---

## 🔧 CUSTOMIZATION

### **Change Logo**

In `NPD-Tracking-Dashboard.html`, find:
```html
<div class="logo">◐</div>
```

Replace with your logo image:
```html
<div class="logo"><img src="sterling-logo.png" style="height: 30px;"></div>
```

### **Update Company Name**

Find `NPD Report Dashboard` in the header and replace with your preference.

### **Add Sterling Logo to Excel**

1. Open `NPD_REPORT.xlsx`
2. Insert → Picture
3. Resize and position in top-left corner
4. Save

---

## 📱 MOBILE ACCESS

The dashboard is fully responsive:

- **Desktop**: Full 3-column layout
- **Tablet**: 2-column grid, stacked sections
- **Mobile**: Single column, horizontal scroll tables

Users can access from phones/tablets - all features work!

---

## 🆘 TROUBLESHOOTING

### **Dashboard shows "No data available"**

**Solutions:**
1. Ensure `NPD_REPORT.xlsx` is in the same folder as the HTML
2. File must be named exactly `NPD_REPORT.xlsx`
3. Try manual upload by dragging file onto upload area

### **Data not updating**

**Solutions:**
1. Save the Excel file
2. Hard refresh browser: `Ctrl+F5` (Windows) or `Cmd+Shift+R` (Mac)
3. Clear browser cache

### **Risk levels seem wrong**

**Remember:**
- Risk is calculated by the system
- If ALL materials are "Received", always shows "ON TRACK"
- Check material statuses in Excel are correct
- Verify launch dates are valid

### **Some projects missing**

**Check:**
1. Excel header row is in row 1
2. No blank rows in data
3. Status values match approved terms exactly (spelling/capitalization)
4. All required columns present

---

## ✅ CHECKLIST: READY FOR PRODUCTION

Before deploying to your team:

- [ ] Sample data replaced with real projects
- [ ] All status values follow approved terminology
- [ ] Dates properly formatted in Excel
- [ ] Tested on Chrome/Edge/Firefox
- [ ] Tested on mobile device
- [ ] SharePoint upload successful (if applicable)
- [ ] Auto-load working
- [ ] Filters tested
- [ ] Risk calculation verified
- [ ] Users trained on features
- [ ] Data manager knows update process
- [ ] Permissions configured (read-only for users)

---

## 📞 SUPPORT & NEXT STEPS

### **Week 1-2: Pilot Phase**
- Deploy to test environment
- Load with real data
- Test with 5-10 users
- Collect feedback

### **Week 3-4: Rollout**
- Train all stakeholders
- Establish data update workflow
- Monitor usage
- Refine as needed

### **Ongoing**
- Regular data updates
- Monthly review meetings
- Quarterly process assessment

---

## 🎓 TRAINING RESOURCES

**For Users:**
- Read SETUP-GUIDE.md - User Guide section
- Watch tutorial (create internal video)
- Practice with sample data

**For Data Managers:**
- Read SETUP-GUIDE.md - Data Update Workflow
- Review Excel requirements
- Practice updates with sample file
- Run through TEST-CASES.md

**For Administrators:**
- Full SETUP-GUIDE.md
- Complete TEST-CASES.md
- SharePoint deployment steps
- Troubleshooting section

---

## 📊 SAMPLE DATA OVERVIEW

The included sample data demonstrates:

**Fragrance Stages:**
- 6 projects in Pending
- 4 projects in Direction Set  
- 8 projects in Finalized

**Risk Levels:**
- High risk projects (launch soon, materials missing)
- Medium risk projects (30-60 days, materials pending)
- On track projects (all materials received)

**Edge Cases:**
- Project with no launch date
- Project with past launch date
- Various material status combinations

**Total:** 18 sample projects covering all scenarios

---

## 🎯 SUCCESS METRICS

The dashboard is successful when:

✅ **Visibility**: Everyone sees same real-time data
✅ **Decision-Making**: Risk levels drive prioritization
✅ **Efficiency**: Updates take minutes, not hours
✅ **Governance**: Single source of truth maintained
✅ **Adoption**: Team refers to dashboard daily
✅ **Accuracy**: Risk calculations trusted as objective
✅ **Collaboration**: Departments coordinate via shared view

---

## 📄 FILE VERSIONS

**Version:** 1.0  
**Date:** February 11, 2026  
**Built for:** Sterling Parfums NPD Department  
**Based on:** Governance Model Specification

---

## 🙏 ACKNOWLEDGMENTS

This dashboard implements the governance framework designed to:
- Eliminate subjective status updates
- Provide objective risk assessment
- Maintain single source of truth
- Enable data-driven decisions
- Improve launch success rate

Built with modern web technologies:
- HTML5
- CSS3 (responsive design)
- JavaScript (ES6+)
- XLSX.js for Excel parsing

---

## 📞 GET HELP

**Questions about:**
- Deployment → See SETUP-GUIDE.md
- Testing → See TEST-CASES.md
- Features → See SETUP-GUIDE.md User Guide section
- Troubleshooting → See SETUP-GUIDE.md Troubleshooting section

---

## 🎉 YOU'RE READY!

Everything you need is in this package:

1. ✅ Dashboard (NPD-Tracking-Dashboard.html)
2. ✅ Sample Data (NPD_REPORT.xlsx)
3. ✅ Setup Instructions (SETUP-GUIDE.md)
4. ✅ Test Cases (TEST-CASES.md)

**Next Steps:**
1. Test locally with sample data (5 min)
2. Replace with your real data (15 min)
3. Deploy to SharePoint (10 min)
4. Share with team (1 min)

**Total time to launch: 30 minutes!**

---

**Built with ❤️ for Sterling Parfums NPD Department**

END OF README
