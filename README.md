# Running Next.js as Administrator on Windows

Some features such as accessing restricted files, binding to privileged
ports, or running system-level commands may require your Next.js
application to run with **Administrator privileges** on Windows.

This guide explains the simplest and safest ways to run your Next.js app
as admin.

------------------------------------------------------------------------

## ✅ 1. Install Dependencies

Run the following inside your project:

``` sh
npm install
```

------------------------------------------------------------------------

## ✅ 2. Start Next.js Normally

For development:

``` sh
npm run dev
```

For production:

``` sh
npm run build
npm start
```

------------------------------------------------------------------------

## ⚙️ 3. Running Next.js as Administrator (Windows)

Windows does not allow Node.js or npm scripts to elevate themselves
directly, so you must launch them from an **Administrator PowerShell**
window.

### **Method A --- Manually Run as Administrator**

1.  Open the **Start Menu**
2.  Search for **PowerShell**
3.  Right-click → **Run as Administrator**
4.  Navigate to your project directory:

``` sh
cd C:\path\to\your\nextjs-project
```

5.  Start the app:

``` sh
npm run dev
```

Your Next.js server is now running with full admin privileges.

------------------------------------------------------------------------

## ⚙️ 4. Method B --- Auto-Elevating PowerShell Script

You can automate the elevation process with a PowerShell script.

Create a file named:

### `run-admin.ps1`

``` powershell
# Auto-elevate this script if not running as Administrator
if (-not ([Security.Principal.WindowsPrincipal] [Security.Principal.WindowsIdentity]::GetCurrent()).IsInRole([Security.Principal.WindowsBuiltInRole] "Administrator")) {
    Start-Process powershell "-File `"$PSCommandPath`"" -Verb RunAs
    exit
}

# Start Next.js
npm run dev
```

Run it:

``` sh
powershell -ExecutionPolicy Bypass -File run-admin.ps1
```

This script will automatically request Administrator permission and then
start your Next.js server.

------------------------------------------------------------------------

## ⚠️ Notes

-   Only use Administrator mode on **your own machine** or in a trusted
    environment.
-   Avoid running development servers as admin for production
    deployments.
-   Be cautious: running Node.js with admin rights gives the app full
    system access.

------------------------------------------------------------------------

## 📄 License

This project is licensed under the MIT License.
