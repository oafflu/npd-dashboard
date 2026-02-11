# NPD TRACKING DASHBOARD - SETUP GUIDE

## 📋 OVERVIEW

This guide provides step-by-step instructions for deploying and using the NPD Tracking Dashboard based on the governance model.

---

## 🚀 DEPLOYMENT OPTIONS

### **Option 1: SharePoint Deployment (Recommended)**

#### Step 1: Prepare Files
1. Download `NPD-Tracking-Dashboard.html`
2. Download `NPD_REPORT.xlsx` (your data file)
3. Ensure both files have the exact names as listed

#### Step 2: Upload to SharePoint
1. Navigate to your SharePoint site
2. Go to the Documents library (or create a new one)
3. Upload both files to the **same folder**
4. The files MUST be in the same directory for auto-load to work

#### Step 3: Configure Permissions
1. Right-click on the folder → Settings → Permissions
2. Ensure users have "Read" access minimum
3. For the Excel file: Grant "Edit" access only to designated data managers (e.g., Kyla)

#### Step 4: Access the Dashboard
1. Click on `NPD-Tracking-Dashboard.html` in SharePoint
2. Select "Open in Browser" (not download)
3. The dashboard will automatically load data from `NPD_REPORT.xlsx`
4. Bookmark the URL for easy access

---

### **Option 2: Local Network Deployment**

#### Step 1: Create Shared Folder
1. Create a shared network folder accessible to all users
2. Example: `\\CompanyServer\NPD-Dashboard\`

#### Step 2: Upload Files
1. Place `NPD-Tracking-Dashboard.html` in the folder
2. Place `NPD_REPORT.xlsx` in the same folder

#### Step 3: Access
1. Users navigate to the network path
2. Double-click `NPD-Tracking-Dashboard.html`
3. Opens in default browser with auto-loaded data

---

### **Option 3: Email Distribution**

#### When to Use
- Quick sharing for review
- Temporary access
- Testing purposes

#### How to Deploy
1. Attach both files to an email
2. Recipients save both files to the **same folder**
3. Open the HTML file in a browser
4. Auto-load will work if files are in the same directory

---

## 📊 EXCEL FILE REQUIREMENTS

### **File Structure**

The Excel file MUST have the following columns (exact spelling required):

```
DCR               - Project identifier (e.g., DCR-2025-2036)
Brand             - Brand name (ARMAF, CDN, FLAVIA, etc.)
Final Name        - Product name
Fragrance Status  - ONLY: "Pending" | "Direction Set" | "Finalized"
Overall Status    - ONLY: "Designing" | "Sampling" | "Approval" | "Approved"
Launch Date       - Date format (MM/DD/YYYY or DD/MM/YYYY)
Bottle Status     - ONLY: "Not Ordered" | "Ordered" | "Received"
Bottle ETA        - Date (only if "Ordered")
Box Status        - ONLY: "Not Ordered" | "Ordered" | "Received"
Box ETA           - Date (only if "Ordered")
Oil Status        - ONLY: "Not Ordered" | "Ordered" | "Received"
Oil ETA           - Date (only if "Ordered")
```

### **CRITICAL RULES**

1. **Fragrance Status** - Must be EXACTLY one of:
   - `Pending`
   - `Direction Set`
   - `Finalized`

2. **Overall Status** - Must be EXACTLY one of:
   - `Designing`
   - `Sampling`
   - `Approval`
   - `Approved`

3. **Material Status** - Must be EXACTLY one of:
   - `Not Ordered`
   - `Ordered`
   - `Received`

4. **Date Format**:
   - Excel date cells (not text)
   - Common formats: `3/15/2025` or `15/3/2025`

5. **No Blank Rows** in the middle of data

6. **Header Row** must be row 1

---

## 🔄 DATA UPDATE WORKFLOW

### **Governance Model Process**

```
1. Department sends update email to Kyla
   ↓
2. Kyla validates information
   ↓
3. Kyla updates NPD_REPORT.xlsx
   ↓
4. Kyla saves file to SharePoint (overwrites old version)
   ↓
5. All users refresh browser to see updated data
```

### **Update Frequency**
- Recommended: Daily or as needed
- Peak periods: Before leadership meetings

### **Data Validation Checklist**
Before saving updates, verify:
- [ ] All statuses use approved values only
- [ ] Dates are properly formatted
- [ ] Material ETAs only populated when status is "Ordered"
- [ ] No duplicate DCR numbers
- [ ] All required columns populated

---

## 🎯 USER GUIDE

### **Dashboard Sections**

#### 1. KPI Cards (Top)
- **Designing**: Projects in artwork & development phase
- **Sampling**: Projects in bottle/box sampling
- **Approval**: Projects awaiting final approval
- **Approved**: Projects ready for production

#### 2. Fragrance Board
- **Pending**: Fragrance not yet identified
- **Direction Set**: Benchmark/direction locked
- **Finalized**: Fragrance approved

Visual Kanban-style view showing progression through fragrance development stages.

#### 3. ETA Tracker
- Filter by risk level using tabs at top
- **High Risk** (Red): Launch ≤30 days with materials missing
- **Medium Risk** (Yellow): Launch 31-60 days with materials missing
- **On Track** (Green): All materials received OR adequate lead time

#### 4. Launches Timeline
- Gantt-style visualization of launch dates
- Color-coded by risk level
- Filter options:
  - All: Show all projects
  - This Month: Current month launches
  - Next 30 Days: Upcoming launches
  - High Risk Only: Critical projects

### **Filters (Sidebar)**

#### Brand Filter
- Select one or multiple brands
- Filters entire dashboard

#### Status Filter
- Filter by Overall Status
- Multiple selection available

#### Search
- Search by DCR, Brand, or Product Name
- Real-time filtering

#### Launch Month
- Filter by launch month/year
- Useful for quarterly planning

### **How Risk is Calculated**

The dashboard automatically calculates risk based on:

**Variables:**
- Days until launch
- Material readiness (Bottle, Box, Oil)

**Logic:**
1. **ON TRACK OVERRIDE**: If all materials are "Received" → Always ON TRACK (green)
2. **HIGH RISK**: Launch ≤30 days AND any material not ready → AT RISK (red)
3. **MEDIUM RISK**: Launch 31-60 days AND any material not ready → WATCH (yellow)
4. **ON TRACK**: Launch 61-90 days AND all materials ordered/received → ON TRACK (green)

**This is system-driven and cannot be overridden manually.**

---

## 🔧 TROUBLESHOOTING

### **Dashboard shows "No data available"**

**Solution 1: Check File Names**
- Excel file must be named exactly: `NPD_REPORT.xlsx`
- Case-sensitive on some systems

**Solution 2: Check File Location**
- Both files must be in the same folder
- For SharePoint: Same document library

**Solution 3: Manual Upload**
- Click "Upload" area in dashboard
- Select your Excel file manually

### **Data not updating after Excel changes**

**Solution:**
1. Save Excel file
2. In dashboard, press `Ctrl+F5` (hard refresh)
3. Or clear browser cache and reload

### **Some projects missing from dashboard**

**Check:**
1. Excel file has proper headers (row 1)
2. No blank rows in data
3. All required columns present
4. Status values match approved list exactly

### **Risk levels seem incorrect**

**Remember:**
- Risk is calculated by the system
- Based on launch date and material status
- If all materials are "Received", always shows "ON TRACK"
- Check that material statuses are correct in Excel

### **Timeline not showing projects**

**Check:**
1. Projects have valid launch dates
2. Timeline filter is not excluding them
3. Try "All" timeline filter first

---

## 📱 MOBILE ACCESS

### **Responsive Design**
- Dashboard automatically adapts to mobile devices
- Tablet: Optimized 2-column layout
- Phone: Single-column stacked layout

### **Mobile Tips**
1. Use landscape mode for better table viewing
2. Filters appear in bottom sheet on mobile
3. Scroll tables horizontally as needed
4. Timeline may require horizontal scrolling

---

## 🔒 SECURITY & PERMISSIONS

### **Recommended Permissions**

**Data Manager (e.g., Kyla):**
- Read & Write access to Excel file
- Read access to HTML file

**All Other Users:**
- Read-only access to both files

**Why?**
- Prevents accidental edits
- Maintains single source of truth
- Ensures data integrity

### **SharePoint Specific**
1. Set folder permissions, not individual file permissions
2. Use SharePoint groups for easier management
3. Enable version history for Excel file
4. Consider approval workflow for major changes

---

## 🎨 CUSTOMIZATION

### **Logo Replacement**
In the HTML file, find line with `class="logo"`:
```html
<div class="logo">◐</div>
```
Replace `◐` with:
- Company logo emoji
- Image tag: `<img src="logo.png" alt="Logo" style="height: 30px;">`

### **Color Scheme**
Not recommended to change as colors align with governance model, but if needed:
- Search for color hex codes (e.g., `#4a69bd`)
- Replace consistently throughout file

### **Company Name**
Find `NPD Report Dashboard` in header and replace

---

## 📊 PERFORMANCE

### **File Size Recommendations**
- Excel file: Up to 1000 rows performs well
- Beyond 1000 rows: Consider archiving old projects

### **Browser Compatibility**
- Chrome: ✅ Recommended
- Edge: ✅ Recommended  
- Firefox: ✅ Supported
- Safari: ✅ Supported
- IE11: ❌ Not supported

---

## 🆘 SUPPORT

### **Common Questions**

**Q: Can I edit data directly in the dashboard?**
A: No. The dashboard is read-only. All updates must be made in the Excel file.

**Q: How often does data refresh?**
A: The dashboard reads the Excel file when you load/refresh the page. After updating Excel, refresh your browser.

**Q: Can multiple people edit the Excel file?**
A: Not recommended. Follow governance model: one data manager updates the file.

**Q: What if I need a new column?**
A: Contact your system administrator. The dashboard is designed for the governance model columns only.

**Q: Can I export dashboard data?**
A: The source data is in the Excel file. Export from there.

---

## ✅ DEPLOYMENT CHECKLIST

Before going live:

- [ ] Excel file has all required columns
- [ ] All status values use approved terms only
- [ ] Sample data tested in dashboard
- [ ] Files uploaded to SharePoint/network location
- [ ] Permissions configured correctly
- [ ] Test users can access successfully
- [ ] Data manager knows update process
- [ ] Users trained on dashboard features
- [ ] Bookmark/shortcut created for easy access
- [ ] Backup process established for Excel file

---

## 🎯 SUCCESS METRICS

**The deployment is successful when:**

1. ✅ All stakeholders can access the dashboard
2. ✅ Data loads automatically without manual upload
3. ✅ KPIs accurately reflect current project status
4. ✅ Risk levels help prioritize urgent items
5. ✅ Fragrance Board shows progression clearly
6. ✅ Filters work as expected
7. ✅ Timeline helps visualize launch schedule
8. ✅ Only designated person edits Excel file
9. ✅ Dashboard becomes the go-to source of truth
10. ✅ Decision-making improves based on data visibility

---

## 📞 NEXT STEPS

1. **Week 1**: Deploy to test environment
2. **Week 2**: Train data manager on Excel updates
3. **Week 3**: Pilot with small user group
4. **Week 4**: Full rollout to all stakeholders
5. **Ongoing**: Collect feedback and refine process

---

**For technical assistance, contact your IT department or dashboard developer.**

END OF SETUP GUIDE
