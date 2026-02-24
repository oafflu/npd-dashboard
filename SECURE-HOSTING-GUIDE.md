SECURE DASHBOARD HOSTING GUIDE
NPD Tracking Dashboard - External Hosting with SharePoint Integration



TABLE OF CONTENTS

1. [Overview](#overview)
2. [Option 1: Microsoft Authentication (RECOMMENDED)](#option-1-microsoft-authentication-recommended)
3. [Option 2: Password Protection (SIMPLE)](#option-2-password-protection-simple)
4. [Testing & Troubleshooting](#testing--troubleshooting)
5. [FAQ](#faq)



OVERVIEW

The Problem
- SharePoint won't execute HTML files properly (shows preview instead)
- Need to host dashboard externally (e.g., Vercel)
- Must restrict access to company members only
- Excel data must stay in SharePoint (single source of truth)
- Dashboard should auto-load Excel without manual uploads

The Solution
Two options to choose from:

| Feature | Option 1: Microsoft Auth | Option 2: Password |
|---------|-------------------------|-------------------|
| Security Level | High             |  Medium |
| Setup Difficulty | Medium (30-45 min) | Easy (10 min) |
| Requires Azure Access | Yes | No |
| Cost | FREE | FREE |
| User Experience | Professional | Simple |
| Access Control | Per-user | Shared password |
| Best For | Production use | Quick testing |



# OPTION 1: Microsoft Authentication (RECOMMENDED)

 What You'll Achieve

After completing this setup:
- ✅ Users sign in with their Microsoft work account
- ✅ Only company members can access dashboard
- ✅ Excel auto-loads from SharePoint
- ✅ No manual file uploads needed
- ✅ Automatic updates when Excel changes
- ✅ Professional authentication experience



PREREQUISITES

Before starting, ensure you have:
- [ ] Access to [Azure Portal](https://portal.azure.com)
- [ ] Admin permissions or IT team contact
- [ ] Dashboard hosted on Vercel (or similar)
- [ ] Excel file in SharePoint
- [ ] 30-45 minutes of time



STEP 1: AZURE APP REGISTRATION

1.1 Access Azure Portal

1. Open browser and go to: https://portal.azure.com
2. Sign in with your company Microsoft account
3. You should see the Azure dashboard

Screenshot guide:
```
Azure Portal Home
├── Search bar at top
├── Left sidebar with services
└── Main dashboard area
```



1.2 Navigate to App Registrations

1. In the **search bar at the top**, type: `App registrations`
2. Click on **App registrations** from the results
3. You'll see a list of registered applications (might be empty)

Alternative path:
- Left sidebar → Azure Active Directory → App registrations



1.3 Create New Registration

1. Click the *+ New registration* button (top left)
2. Fill in the form:

Name:
```
NPD Dashboard
```
*(You can name it anything - this is just the internal name)*

Supported account types:
- Select: *"Accounts in this organizational directory only (Single tenant)"*
- This ensures ONLY your company members can sign in

Redirect URI:
- Platform: Select *"Single-page application (SPA)"*
- URL: Enter your Vercel URL + `/callback`

  Example:
  ```
  https://npd-dashboard.vercel.app/callback
  ```

  ⚠️ *IMPORTANT:* Replace `npd-dashboard.vercel.app` with YOUR actual Vercel URL!

3. Click **Register** button at the bottom



 1.4 Copy Your Application (Client) ID

After registration, you'll see the app overview page.

1. Look for *Application (client) ID* - it looks like:
   ```
   12345678-1234-1234-1234-123456789abc
   ```

2. Click the **copy icon** next to it

3. *SAVE THIS SOMEWHERE SAFE* - you'll need it later

Where to save:
- Create a text file called `azure-credentials.txt`
- Paste it like this:
  ```
  Application (client) ID: 12345678-1234-1234-1234-123456789abc
  ```



1.5 Create Client Secret

1. In the left sidebar, click *Certificates & secrets*
2. Click *+ New client secret*
3. Fill in:
   - *Description:* `NPD Dashboard Secret`
   - *Expires:* Choose `24 months` (or as per your company policy)
4. Click *Add*

5. *IMMEDIATELY* copy the *Value* (not the Secret ID!)
   - It looks like: `abc123~XYZ789.def456-ghi789`
   - ⚠️ *YOU CAN ONLY SEE THIS ONCE!*
   - If you navigate away, you'll have to create a new one

6. *SAVE THIS IN YOUR TEXT FILE:*
   ```
   Application (client) ID: 12345678-1234-1234-1234-123456789abc
   Client Secret: abc123~XYZ789.def456-ghi789
   ```



1.6 Set API Permissions

1. In the left sidebar, click *API permissions*
2. Click *+ Add a permission*
3. Click *Microsoft Graph*
4. Click *Delegated permissions*
5. In the search box, type: `Files.Read.All`
6. Check the box next to *Files.Read.All*
7. Click *Add permissions* at the bottom

8. *IMPORTANT:* Click *Grant admin consent for [Your Company]*
   - If you don't see this button, ask your IT admin to do it
   - This allows users to access SharePoint files



1.7 Note Your Tenant ID

1. Go back to the *Overview* page (click "Overview" in left sidebar)
2. Find *Directory (tenant) ID* - it looks like:
   ```
   87654321-4321-4321-4321-210987654321
   ```
3. Copy it and *SAVE IN YOUR TEXT FILE:*
   ```
   Application (client) ID: 12345678-1234-1234-1234-123456789abc
   Client Secret: abc123~XYZ789.def456-ghi789
   Tenant ID: 87654321-4321-4321-4321-210987654321
   ```

*CHECKPOINT:* You should now have 3 values saved in your text file.



STEP 2: GET SHAREPOINT EXCEL FILE PATH

2.1 Open Your Excel File in SharePoint

1. Go to your SharePoint site
2. Navigate to the folder containing `NPD_REPORT.xlsx`
3. *Right-click* on the file → *Details*



2.2 Get the SharePoint Site URL

The URL in your browser looks like:
```
https://yourcompany.sharepoint.com/sites/yoursite/Shared%20Documents/NPD_REPORT.xlsx
```

Break it down:
- *Site URL:* `https://yourcompany.sharepoint.com/sites/yoursite`
- *File Path:* `/Shared Documents/NPD_REPORT.xlsx`

*SAVE THESE:*
```
SharePoint Site URL: https://yourcompany.sharepoint.com/sites/yoursite
Excel File Path: /Shared Documents/NPD_REPORT.xlsx
```


2.3 Alternative: Get Direct Download Link

Easier method:

1. In SharePoint, click the *three dots (...)* next to your Excel file
2. Click *Details* (right panel opens)
3. Scroll down to find *Path*
4. Copy the entire path

OR:

1. Click *Share* on the Excel file
2. Change permissions to "People in your organization"
3. Copy the link
4. The link contains your file information



STEP 3: UPDATE DASHBOARD CODE

3.1 Download Required Libraries

You need to add Microsoft Authentication Library (MSAL) to your dashboard.

*Open your `NPD-Tracking-Dashboard.html` file in a text editor.*



3.2 Add MSAL Library

*FIND THIS LINE* (near the top of the file, around line 6):
```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
```

*ADD THIS LINE IMMEDIATELY AFTER IT:*
```html
<script src="https://alcdn.msauth.net/browser/2.38.1/js/msal-browser.min.js"></script>
```

*Example of how it should look:*
```html
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NPD Tracking Dashboard</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    <script src="https://alcdn.msauth.net/browser/2.38.1/js/msal-browser.min.js"></script>
    <style>
```



3.3 Add Azure Configuration

*FIND THIS SECTION* (search for "CONFIGURATION" - around line 1730):
```javascript
// ============================================
// CONFIGURATION - UPDATE THIS SECTION
// ============================================
```

*REPLACE THE ENTIRE CONFIGURATION SECTION WITH:*

```javascript
// ============================================
// CONFIGURATION - UPDATE THIS SECTION
// ============================================

// MICROSOFT AUTHENTICATION CONFIGURATION
const msalConfig = {
    auth: {
        clientId: "PASTE_YOUR_CLIENT_ID_HERE",  // From Step 1.4
        authority: "https://login.microsoftonline.com/PASTE_YOUR_TENANT_ID_HERE",  // From Step 1.7
        redirectUri: "https://your-vercel-app.vercel.app/callback"  // Your Vercel URL + /callback
    },
    cache: {
        cacheLocation: "sessionStorage",
        storeAuthStateInCookie: false
    }
};

// SHAREPOINT EXCEL CONFIGURATION
const SHAREPOINT_CONFIG = {
    siteUrl: "https://yourcompany.sharepoint.com/sites/yoursite",  // From Step 2.2
    filePath: "/Shared Documents/NPD_REPORT.xlsx"  // From Step 2.2
};

// Initialize MSAL
const msalInstance = new msal.PublicClientApplication(msalConfig);

// Microsoft Graph API scopes
const loginRequest = {
    scopes: ["Files.Read.All", "Sites.Read.All"]
};
```

*NOW REPLACE THE PLACEHOLDER VALUES:*

1. Replace `PASTE_YOUR_CLIENT_ID_HERE` with your Application (client) ID from Step 1.4
2. Replace `PASTE_YOUR_TENANT_ID_HERE` with your Tenant ID from Step 1.7
3. Replace `https://your-vercel-app.vercel.app/callback` with your actual Vercel URL + `/callback`
4. Replace `https://yourcompany.sharepoint.com/sites/yoursite` with your SharePoint site URL
5. Replace `/Shared Documents/NPD_REPORT.xlsx` with your Excel file path

*Example of completed configuration:*
```javascript
const msalConfig = {
    auth: {
        clientId: "12345678-1234-1234-1234-123456789abc",
        authority: "https://login.microsoftonline.com/87654321-4321-4321-4321-210987654321",
        redirectUri: "https://npd-dashboard.vercel.app/callback"
    },
    cache: {
        cacheLocation: "sessionStorage",
        storeAuthStateInCookie: false
    }
};

const SHAREPOINT_CONFIG = {
    siteUrl: "https://sterlingparfums.sharepoint.com/sites/NPDTeam",
    filePath: "/Shared Documents/NPD_REPORT.xlsx"
};
```

---

3.4 Add Authentication Functions

*FIND THIS LINE* (search for "window.addEventListener('load'" - around line 1755):
```javascript
window.addEventListener('load', function() {
    autoLoadExcel();
});
```

*REPLACE IT WITH:*

```javascript
// ============================================
// AUTHENTICATION & AUTO-LOAD
// ============================================

window.addEventListener('load', async function() {
    await handleAuthentication();
});

async function handleAuthentication() {
    try {
        // Check if returning from redirect
        const response = await msalInstance.handleRedirectPromise();

        if (response) {
            // User just signed in
            console.log("Sign-in successful!");
            await loadExcelFromSharePoint();
        } else {
            // Check if user is already signed in
            const accounts = msalInstance.getAllAccounts();

            if (accounts.length > 0) {
                // User already signed in
                msalInstance.setActiveAccount(accounts[0]);
                await loadExcelFromSharePoint();
            } else {
                // User not signed in - show sign in button
                showSignInButton();
            }
        }
    } catch (error) {
        console.error("Authentication error:", error);
        showError("Authentication failed. Please try again.");
    }
}

function showSignInButton() {
    // Hide the upload section
    document.querySelector('.upload-section').style.display = 'none';

    // Create and show sign-in button
    const mainContent = document.querySelector('.main-content');
    const signInDiv = document.createElement('div');
    signInDiv.id = 'sign-in-container';
    signInDiv.style.cssText = 'text-align: center; padding: 4rem 2rem; background: white; border-radius: 12px; box-shadow: 0 2px 15px rgba(0,0,0,0.1); margin: 2rem auto; max-width: 500px;';
    signInDiv.innerHTML = `
        <div style="font-size: 3rem; margin-bottom: 1rem;">🔐</div>
        <h2 style="color: #2c3e50; margin-bottom: 1rem;">NPD Dashboard</h2>
        <p style="color: #7f8c8d; margin-bottom: 2rem;">Please sign in with your company Microsoft account to access the dashboard.</p>
        <button onclick="signIn()" style="background: #4a69bd; color: white; border: none; padding: 1rem 2rem; font-size: 1rem; border-radius: 8px; cursor: pointer; font-weight: 600; transition: background 0.3s;">
            Sign in with Microsoft
        </button>
    `;

    mainContent.insertBefore(signInDiv, mainContent.firstChild);
}

async function signIn() {
    try {
        await msalInstance.loginRedirect(loginRequest);
    } catch (error) {
        console.error("Sign-in error:", error);
        showError("Sign-in failed. Please try again.");
    }
}

async function loadExcelFromSharePoint() {
    try {
        // Show loading message
        showLoading("Loading dashboard data...");

        // Get access token
        const account = msalInstance.getActiveAccount();
        const tokenResponse = await msalInstance.acquireTokenSilent({
            ...loginRequest,
            account: account
        });

        // Construct Microsoft Graph API URL
        const graphUrl = await getGraphApiUrl();

        // Fetch Excel file from SharePoint
        const response = await fetch(graphUrl, {
            headers: {
                'Authorization': `Bearer ${tokenResponse.accessToken}`
            }
        });

        if (!response.ok) {
            throw new Error(`Failed to load Excel file: ${response.statusText}`);
        }

        // Convert to array buffer and process
        const arrayBuffer = await response.arrayBuffer();
        processExcelFile(arrayBuffer);

        // Hide loading, show upload section
        hideLoading();
        document.querySelector('.upload-section').style.display = 'block';

        // Add sign-out button to header
        addSignOutButton(account);

    } catch (error) {
        console.error("Error loading Excel from SharePoint:", error);

        // If token expired, try interactive sign-in
        if (error.name === "InteractionRequiredAuthError") {
            await msalInstance.acquireTokenRedirect(loginRequest);
        } else {
            hideLoading();
            showError("Failed to load dashboard data. Please refresh the page or contact support.");
        }
    }
}

async function getGraphApiUrl() {
    // Convert SharePoint URL to Graph API format
    const siteUrl = SHAREPOINT_CONFIG.siteUrl;
    const filePath = SHAREPOINT_CONFIG.filePath;

    // Extract site path from URL
    const urlParts = siteUrl.split('.sharepoint.com');
    const hostname = urlParts[0].split('//')[1]; // e.g., "yourcompany"
    const sitePath = urlParts[1]; // e.g., "/sites/yoursite"

    // Get access token for initial request
    const account = msalInstance.getActiveAccount();
    const tokenResponse = await msalInstance.acquireTokenSilent({
        ...loginRequest,
        account: account
    });

    // Get site ID using Graph API
    const siteInfoUrl = `https://graph.microsoft.com/v1.0/sites/${hostname}.sharepoint.com:${sitePath}`;
    const siteResponse = await fetch(siteInfoUrl, {
        headers: { 'Authorization': `Bearer ${tokenResponse.accessToken}` }
    });
    const siteData = await siteResponse.json();
    const siteId = siteData.id;

    // Construct file URL
    const encodedPath = encodeURIComponent(filePath);
    return `https://graph.microsoft.com/v1.0/sites/${siteId}/drive/root:${filePath}:/content`;
}

function addSignOutButton(account) {
    // Add user info and sign-out to header
    const headerRight = document.querySelector('.header-right');

    // Remove existing sign-out if present
    const existing = document.getElementById('user-info');
    if (existing) existing.remove();

    const userInfo = document.createElement('div');
    userInfo.id = 'user-info';
    userInfo.style.cssText = 'display: flex; align-items: center; gap: 1rem;';
    userInfo.innerHTML = `
        <span style="color: white; font-size: 0.9rem;">${account.username}</span>
        <button onclick="signOut()" style="background: rgba(255,255,255,0.2); color: white; border: none; padding: 0.5rem 1rem; border-radius: 6px; cursor: pointer; font-size: 0.9rem; transition: background 0.3s;" onmouseover="this.style.background='rgba(255,255,255,0.3)'" onmouseout="this.style.background='rgba(255,255,255,0.2)'">
            Sign Out
        </button>
    `;

    headerRight.appendChild(userInfo);
}

function signOut() {
    const logoutRequest = {
        account: msalInstance.getActiveAccount()
    };

    msalInstance.logoutRedirect(logoutRequest);
}

function showLoading(message) {
    const existing = document.getElementById('loading-overlay');
    if (existing) existing.remove();

    const overlay = document.createElement('div');
    overlay.id = 'loading-overlay';
    overlay.style.cssText = 'position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: rgba(0,0,0,0.7); display: flex; align-items: center; justify-content: center; z-index: 9999;';
    overlay.innerHTML = `
        <div style="background: white; padding: 2rem 3rem; border-radius: 12px; text-align: center;">
            <div style="font-size: 2rem; margin-bottom: 1rem;">⏳</div>
            <div style="font-size: 1.2rem; color: #2c3e50; font-weight: 600;">${message}</div>
        </div>
    `;
    document.body.appendChild(overlay);
}

function hideLoading() {
    const overlay = document.getElementById('loading-overlay');
    if (overlay) overlay.remove();
}

function showError(message) {
    alert(message); // Simple alert for now - you can enhance this
}
```



3.5 Update Auto-Load Function

*FIND THIS FUNCTION* (search for "async function autoLoadExcel" - around line 1765):
```javascript
async function autoLoadExcel() {
    try {
        const response = await fetch('NPD_REPORT.xlsx');
        if (response.ok) {
            const arrayBuffer = await response.arrayBuffer();
            processExcelFile(arrayBuffer);
        }
    } catch (error) {
        console.log('No auto-load file found');
    }
}
```

*REPLACE IT WITH:*
```javascript
// Auto-load function - now only used as fallback for manual upload
async function autoLoadExcel() {
    // This function is now handled by loadExcelFromSharePoint()
    // Keep it for backward compatibility with manual upload feature
    try {
        const response = await fetch('NPD_REPORT.xlsx');
        if (response.ok) {
            const arrayBuffer = await response.arrayBuffer();
            processExcelFile(arrayBuffer);
        }
    } catch (error) {
        console.log('Manual file upload available');
    }
}
```



STEP 4: DEPLOY TO VERCEL

4.1 Upload to Vercel

1. Go to [Vercel](https://vercel.com)
2. Sign in with your GitHub account (or create one)
3. Click **Add New Project**
4. Upload your `NPD-Tracking-Dashboard.html` file
5. Vercel will give you a URL like: `https://npd-dashboard.vercel.app`



4.2 Update Redirect URI in Azure

*IMPORTANT:* Go back to Azure Portal!

1. Azure Portal → App registrations → Your app (NPD Dashboard)
2. Left sidebar → *Authentication*
3. Under "Single-page application", click *Add URI*
4. Enter your EXACT Vercel URL + `/callback`:
   ```
   https://npd-dashboard.vercel.app/callback
   ```
5. Click *Save*



STEP 5: TEST THE DASHBOARD

5.1 First Time Access

1. Open your Vercel URL in browser
2. You should see a "Sign in with Microsoft" button
3. Click the button
4. Microsoft sign-in page appears
5. Sign in with your company account
6. You may see a permissions consent screen - click **Accept**
7. Dashboard should load with data from SharePoint!



5.2 What Users Will Experience

*First time:*
- See sign-in page
- Click "Sign in with Microsoft"
- Sign in with company account
- Dashboard loads

*Next time:*
- Open dashboard URL
- Automatically signed in (if session active)
- Dashboard loads immediately



STEP 6: SHARE WITH TEAM

1. Share your Vercel URL with team members:
   ```
   https://npd-dashboard.vercel.app
   ```

2. Tell them:
   - Click "Sign in with Microsoft"
   - Use their company email
   - Dashboard will load automatically

3. *Security Note:*
   - Only people with company Microsoft accounts can sign in
   - Excel file never leaves SharePoint
   - Each user accesses Excel with their own SharePoint permissions



OPTION 1 COMPLETE!

*What you've achieved:*
-  Dashboard hosted on Vercel
-  Microsoft authentication required
-  Only company members can access
-  Excel auto-loads from SharePoint
-  No manual uploads needed
-  Secure and professional





OPTION 2: Password Protection (SIMPLE)

What You'll Achieve

After completing this setup:
-  Dashboard protected with password
-  Excel auto-loads from SharePoint
-  No Azure setup needed
-  Quick to implement (10 minutes)

* LIMITATIONS:*
- Less secure (shared password)
- Anyone with password can access
- No user tracking



STEP 1: GET SHAREPOINT EXCEL DIRECT LINK

1.1 Create Shareable Link

1. In SharePoint, *right-click* your `NPD_REPORT.xlsx` file
2. Click *Share*
3. In the sharing dialog:
   - Change permission to *"People in [Your Organization] with the link can view"*
   - Click *Copy link*

4. The link looks like:
   ```
   https://yourcompany.sharepoint.com/:x:/s/site/hash/file.xlsx?parameters
   ```



1.2 Convert to Direct Download Link

Take your link and modify it:

*Original:*
```
https://yourcompany.sharepoint.com/:x:/s/yoursite/EabcXYZ123/NPD_REPORT.xlsx?d=abc123
```

*Modified (for download):*
```
https://yourcompany.sharepoint.com/:x:/s/yoursite/EabcXYZ123/NPD_REPORT.xlsx?download=1
```

*Change:* Replace `?d=` with `?download=1` at the end

*SAVE THIS URL* - you'll need it in Step 2.



## STEP 2: UPDATE DASHBOARD CODE

### 2.1 Add Password Protection

**Open your `NPD-Tracking-Dashboard.html` file in a text editor.**

**FIND THIS SECTION** (search for "window.addEventListener('load'" - around line 1755):
```javascript
window.addEventListener('load', function() {
    autoLoadExcel();
});
```

*REPLACE IT WITH:*

```javascript
// ============================================
// PASSWORD PROTECTION CONFIGURATION
// ============================================

// CHANGE THIS PASSWORD to whatever you want
const DASHBOARD_PASSWORD = "Sterling2026!";

// SharePoint Excel direct download link
const SHAREPOINT_EXCEL_URL = "PASTE_YOUR_SHAREPOINT_LINK_HERE";

// ============================================
// PASSWORD PROTECTION LOGIC
// ============================================

window.addEventListener('load', function() {
    checkPassword();
});

function checkPassword() {
    // Check if already authenticated in this session
    const isAuthenticated = sessionStorage.getItem('dashboard_auth');

    if (isAuthenticated === 'true') {
        // Already authenticated - load dashboard
        loadDashboard();
    } else {
        // Show password prompt
        showPasswordPrompt();
    }
}

function showPasswordPrompt() {
    // Hide main content
    document.querySelector('.main-content').style.display = 'none';

    // Create password prompt
    const passwordDiv = document.createElement('div');
    passwordDiv.id = 'password-container';
    passwordDiv.style.cssText = 'position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); display: flex; align-items: center; justify-content: center; z-index: 10000;';
    passwordDiv.innerHTML = `
        <div style="background: white; padding: 3rem; border-radius: 16px; box-shadow: 0 10px 40px rgba(0,0,0,0.3); max-width: 400px; width: 90%;">
            <div style="text-align: center; margin-bottom: 2rem;">
                <div style="font-size: 4rem; margin-bottom: 1rem;">🔐</div>
                <h2 style="color: #2c3e50; margin-bottom: 0.5rem;">NPD Dashboard</h2>
                <p style="color: #7f8c8d; font-size: 0.9rem;">Enter password to access</p>
            </div>

            <div style="margin-bottom: 1.5rem;">
                <input type="password" id="password-input" placeholder="Enter password"
                       style="width: 100%; padding: 1rem; border: 2px solid #ecf0f1; border-radius: 8px; font-size: 1rem; transition: border-color 0.3s;"
                       onkeypress="if(event.key==='Enter') verifyPassword()"
                       onfocus="this.style.borderColor='#4a69bd'"
                       onblur="this.style.borderColor='#ecf0f1'">
            </div>

            <button onclick="verifyPassword()"
                    style="width: 100%; background: #4a69bd; color: white; border: none; padding: 1rem; font-size: 1rem; border-radius: 8px; cursor: pointer; font-weight: 600; transition: background 0.3s;"
                    onmouseover="this.style.background='#3d5a9c'"
                    onmouseout="this.style.background='#4a69bd'">
                Access Dashboard
            </button>

            <div id="error-message" style="color: #e74c3c; margin-top: 1rem; text-align: center; font-size: 0.9rem; display: none;">
                Incorrect password. Please try again.
            </div>
        </div>
    `;

    document.body.appendChild(passwordDiv);

    // Focus on password input
    setTimeout(() => {
        document.getElementById('password-input').focus();
    }, 100);
}

function verifyPassword() {
    const input = document.getElementById('password-input').value;
    const errorMsg = document.getElementById('error-message');

    if (input === DASHBOARD_PASSWORD) {
        // Correct password
        sessionStorage.setItem('dashboard_auth', 'true');

        // Remove password prompt
        const container = document.getElementById('password-container');
        container.style.opacity = '0';
        container.style.transition = 'opacity 0.3s';
        setTimeout(() => {
            container.remove();
            loadDashboard();
        }, 300);
    } else {
        // Wrong password
        errorMsg.style.display = 'block';
        document.getElementById('password-input').value = '';
        document.getElementById('password-input').focus();

        // Shake animation
        const container = document.getElementById('password-container').firstElementChild;
        container.style.animation = 'shake 0.5s';
        setTimeout(() => {
            container.style.animation = '';
        }, 500);
    }
}

// Add shake animation
const style = document.createElement('style');
style.textContent = `
    @keyframes shake {
        0%, 100% { transform: translateX(0); }
        10%, 30%, 50%, 70%, 90% { transform: translateX(-10px); }
        20%, 40%, 60%, 80% { transform: translateX(10px); }
    }
`;
document.head.appendChild(style);

function loadDashboard() {
    // Show main content
    document.querySelector('.main-content').style.display = 'block';

    // Load Excel from SharePoint
    loadExcelFromSharePoint();
}

async function loadExcelFromSharePoint() {
    try {
        // Show loading
        showLoading("Loading dashboard data from SharePoint...");

        // Fetch Excel from SharePoint using direct link
        const response = await fetch(SHAREPOINT_EXCEL_URL);

        if (!response.ok) {
            throw new Error('Failed to load Excel file from SharePoint');
        }

        // Convert to array buffer and process
        const arrayBuffer = await response.arrayBuffer();
        processExcelFile(arrayBuffer);

        hideLoading();

    } catch (error) {
        console.error('Error loading Excel from SharePoint:', error);
        hideLoading();
        alert('Failed to load dashboard data. Please check your internet connection and try refreshing the page.');
    }
}

function showLoading(message) {
    const existing = document.getElementById('loading-overlay');
    if (existing) existing.remove();

    const overlay = document.createElement('div');
    overlay.id = 'loading-overlay';
    overlay.style.cssText = 'position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: rgba(0,0,0,0.7); display: flex; align-items: center; justify-content: center; z-index: 9999;';
    overlay.innerHTML = `
        <div style="background: white; padding: 2rem 3rem; border-radius: 12px; text-align: center;">
            <div style="font-size: 2rem; margin-bottom: 1rem;">⏳</div>
            <div style="font-size: 1.2rem; color: #2c3e50; font-weight: 600;">${message}</div>
        </div>
    `;
    document.body.appendChild(overlay);
}

function hideLoading() {
    const overlay = document.getElementById('loading-overlay');
    if (overlay) overlay.remove();
}
```



2.2 Configure Your Password and SharePoint Link

In the code you just pasted, *FIND THESE TWO LINES:*

```javascript
const DASHBOARD_PASSWORD = "Sterling2026!";
const SHAREPOINT_EXCEL_URL = "PASTE_YOUR_SHAREPOINT_LINK_HERE";
```

*CHANGE THEM:*

1. *Set your password:*
   ```javascript
   const DASHBOARD_PASSWORD = "YourSecurePassword123!";
   ```
   Choose a strong password and share it only with your team.

2. *Paste your SharePoint link from Step 1:*
   ```javascript
   const SHAREPOINT_EXCEL_URL = "https://yourcompany.sharepoint.com/:x:/s/yoursite/EabcXYZ123/NPD_REPORT.xlsx?download=1";
   ```

*Example of completed configuration:*
```javascript
const DASHBOARD_PASSWORD = "Sterling2026!";
const SHAREPOINT_EXCEL_URL = "https://sterlingparfums.sharepoint.com/:x:/s/NPDTeam/Eabc123XYZ/NPD_REPORT.xlsx?download=1";
```



2.3 Remove Old Auto-Load Function

*FIND THIS FUNCTION* (search for "async function autoLoadExcel"):
```javascript
async function autoLoadExcel() {
    try {
        const response = await fetch('NPD_REPORT.xlsx');
        if (response.ok) {
            const arrayBuffer = await response.arrayBuffer();
            processExcelFile(arrayBuffer);
        }
    } catch (error) {
        console.log('No auto-load file found');
    }
}
```

*DELETE THE ENTIRE FUNCTION* (or comment it out by adding `//` at the start of each line)

*Why?* The new `loadExcelFromSharePoint()` function handles this now.


STEP 3: DEPLOY TO VERCEL

3.1 Upload Updated Dashboard

1. Go to [Vercel](https://vercel.com)
2. Sign in (or create free account)
3. Click *New Project*
4. Upload your updated `NPD-Tracking-Dashboard.html`
5. Deploy!

You'll get a URL like: `https://npd-dashboard.vercel.app`



STEP 4: TEST THE DASHBOARD

4.1 First Access

1. Open your Vercel URL in a browser
2. You should see a password prompt with a lock icon
3. Enter the password you set in Step 2.2
4. Click "Access Dashboard"
5. Dashboard should load with Excel data from SharePoint!



4.2 What Users Will Experience

*First time in a session:*
- See password prompt
- Enter password
- Dashboard loads

*After entering password:*
- Password remembered for that browser session
- Closing browser = need to enter password again
- This is a security feature



STEP 5: SHARE WITH TEAM

5.1 Share Access Instructions

Send this to your team:

```
NPD Dashboard Access

URL: https://npd-dashboard.vercel.app
Password: [Your Password Here]

Instructions:
1. Open the URL in your browser
2. Enter the password when prompted
3. Dashboard will load automatically

The dashboard auto-refreshes data from SharePoint.
Close your browser when done to ensure security.
```



5.2 Password Security Tips

- Don't share the password via email - use a secure method (Teams message, in-person, etc.)
- Change the password regularly (every 3-6 months)
- Use a strong password with letters, numbers, and symbols
- Keep track of who has the password



 OPTION 2 COMPLETE!

What you've achieved:
-  Dashboard hosted on Vercel
-  Password protection
-  Excel auto-loads from SharePoint
-  No manual uploads needed
-  Quick and simple setup





TESTING & TROUBLESHOOTING

Common Issues & Solutions

Issue: "Sign-in failed" (Option 1)

*Solutions:*
1. Check Application (client) ID is correct in code
2. Verify Tenant ID is correct
3. Ensure Redirect URI in Azure matches your Vercel URL exactly
4. Make sure admin consent was granted in Azure



Issue: "Failed to load Excel" (Both Options)

*Solutions:*
1. Check SharePoint permissions: Open the Excel file in SharePoint - if you can see it, the link is good
2. Verify the link: Copy the SharePoint URL again and update the code
3. Check your internet connection
4. For Option 2: Make sure the link ends with `?download=1`


Issue: Password not working (Option 2)

*Solutions:*
1. Check for typos in password (case-sensitive!)
2. Make sure you saved the HTML file after changing the password
3. Clear browser cache and try again
4. Re-deploy to Vercel with the updated file



Issue: "CORS error" in browser console

*Solution for Option 2:*
The SharePoint link must be a "shareable link" (created via the Share button), not a direct browser URL. The link should contain special parameters that allow cross-origin access.



Issue: Dashboard loads but no data shows

*Solutions:*
1. Check browser console for errors (F12 → Console tab)
2. Verify Excel file has data in the correct format
3. Check column names match exactly what dashboard expects
4. Try uploading the Excel file manually to test if it's a loading issue



Testing Checklist

Option 1 Testing

-  Sign-in button appears on first visit
-  Clicking sign-in redirects to Microsoft login
-  After signing in, dashboard loads with data
-  User email appears in header
-  Sign-out button works
-  Closing browser and reopening auto-signs in (if session active)
-  Team members can sign in with their accounts
-  Non-company emails cannot sign in



 Option 2 Testing

-  Password prompt appears on first visit
-  Correct password grants access
-  Wrong password shows error message
-  Dashboard loads with SharePoint data
-  Password remembered in same browser session
-  Closing browser requires password re-entry
-  Shared password works for team members



Performance Tips

Speed Up Dashboard Loading

1. Reduce image file sizes in SharePoint
2. Minimize Excel file size (remove unused data)
3. Use browser caching (Vercel does this automatically)



Security Best Practices

Option 1 (Microsoft Auth):
- Review who has access in Azure AD regularly
- Monitor sign-in logs in Azure Portal
- Revoke access for former employees in Azure AD
- Enable multi-factor authentication (MFA) company-wide

Option 2 (Password):
- Change password every 3-6 months
- Don't share via email or unsecured channels
- Track who has the password
- Use a password manager if sharing with large team



 FAQ

 General Questions

Q: Which option should I choose?

A:
- Option 1 if you want maximum security and have Azure access
- Option 2 if you need something working quickly without IT involvement



Q: Can I switch from Option 2 to Option 1 later?

A: Yes! You can always upgrade. Just follow Option 1 steps and replace the code.



Q: How much does this cost?

A:
- Vercel: Free tier is sufficient
- Azure AD: Free (included with Microsoft 365)
- SharePoint: You're already using it
- Total: $0



Q: What happens if Excel file is updated in SharePoint?

A: Users just need to refresh the dashboard (F5) to see the latest data. No re-upload needed!



Q: Can I use this with Google Sheets instead?

A: Not with these exact instructions, but the concept is similar. You'd need Google OAuth instead of Microsoft.



Option 1 Specific

Q: What if I don't have Azure Portal access?

A: Ask your IT department to create the App Registration for you. Give them this guide.



Q: How do I remove someone's access?

A: Their access is controlled by Azure AD. Remove them from your company's Azure AD and they lose access automatically.



Q: Can external consultants access?

A: Only if they have guest accounts in your Azure AD. Your IT can set this up.



Option 2 Specific

Q: How secure is password protection?

A: Better than no protection, but not as secure as Option 1. Good for internal team use, not for sensitive financial data.



Q: Can I have different passwords for different users?

A: Not with this simple setup. For per-user access control, use Option 1.



Q: What if someone shares the password publicly?

A: You'd need to immediately:
1. Change the password in the code
2. Re-deploy to Vercel
3. Share new password with team only



Technical Questions

Q: Does the Excel file need to be in a specific format?

A: Yes, it should have the columns your dashboard expects. The structure must match what's defined in the `processExcelFile()` function.



Q: Can I customize the sign-in page?

A:
- Option 1: Limited - Microsoft controls the sign-in page
- Option 2: Yes - edit the HTML in `showPasswordPrompt()` function



Q: What browsers are supported?

A: All modern browsers:
- Chrome ✅
- Edge ✅
- Firefox ✅
- Safari ✅
- Internet Explorer ❌ (not supported)



Q: Can I use this on mobile?

A: Yes! Both options work on mobile devices. The dashboard is responsive.



Q: What if SharePoint is down?

A: Dashboard won't be able to load data. You'll see an error message. Users would need to wait for SharePoint to come back online.



Advanced Questions

Q: Can I add more authentication factors?

A:
- Option 1: Yes - enable MFA in Azure AD (affects all Microsoft sign-ins)
- Option 2: No - would require more complex implementation



Q: Can I track who accessed the dashboard?

A:
- Option 1: Yes - check Azure AD sign-in logs
- Option 2: No - all users use same password



Q: Can I host on something other than Vercel?

A: Yes! Any static hosting service works:
- Netlify
- GitHub Pages
- AWS S3 + CloudFront
- Azure Static Web Apps
Just update the Redirect URI in Azure (Option 1) accordingly.





SUPPORT & NEXT STEPS

Getting Help

If you encounter issues:

1. Check the Troubleshooting section above
2. Review browser console (F12 → Console) for error messages
3. Verify all URLs and IDs are correct in the code
4. Test with a different browser
5. Contact your IT department for Azure/SharePoint issues



Enhancements You Can Add Later

1. Auto-refresh: Make dashboard reload Excel data every 5 minutes
2. Export feature: Allow users to download current data as PDF
3. Email notifications: Alert team when risk levels change
4. User preferences: Let users save their filter selections
5. Mobile app: Convert to a Progressive Web App (PWA)



Maintenance

Monthly Tasks
-  Verify dashboard is loading correctly
-  Check for any errors in browser console
-  Review who has access (Option 1)
-  Test with updated Excel data

Quarterly Tasks
-  Update password (Option 2)
-  Review Azure AD permissions (Option 1)
-  Check Vercel deployment status
-  Collect user feedback



Feedback & Improvements

Keep track of:
- User complaints or issues
- Feature requests
- Performance problems
- Security concerns

This will help you improve the dashboard over time!





APPENDIX

Code Backup Locations

Before making changes, save these lines from your original dashboard:

Important to backup:
1. The entire `<script>` section
2. Your custom styling (if any)
3. Any modifications you made to KPI calculations
4. Custom filters or features

How to backup:
1. Copy entire `NPD-Tracking-Dashboard.html`
2. Save as `NPD-Tracking-Dashboard-BACKUP.html`
3. Keep in safe location



Quick Reference

Option 1 - Files Needed
- Application (client) ID
- Tenant ID
- Client Secret
- SharePoint Site URL
- Excel File Path
- Vercel URL

Option 2 - Files Needed
- Dashboard Password (your choice)
- SharePoint Excel Direct Link
- Vercel URL



Version History

Version 1.0 - Initial secure hosting setup
- Microsoft Authentication (Option 1)
- Password Protection (Option 2)
- SharePoint integration
- Auto-load from SharePoint





Note: Security is important, but  Option 2 gets you up and running quickly, and you can always upgrade to Option 1 later when you have Azure access!
