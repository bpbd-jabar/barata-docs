# Test Plan: BARATA System Re-engineering

## 1. Objectives
*   **Validate Feature Parity:** Ensure the new system behaves identically to the legacy system for core workflows.
*   **Baseline Performance:** Measure current response times (especially for the map and chat) to ensure the new stack improves performance.
*   **Data Migration Integrity:** Verify that 18,000+ records are migrated without loss or corruption.
*   **Automation First:** Create a reusable E2E suite to be used during development and after deployment.

## 2. Scope of Testing
### A. Core Functional Modules
1.  **Public Dashboard:** Map interactivity, filters, and info overlays.
2.  **Infografis Bencana:** Accuracy of aggregated data (deaths, injuries, damage counts).
3.  **Input Data Bencana:** Form validations, rich text inputs, and Leaflet coordinate picking.
4.  **BARATA CHAT (Laporan Kab/Kota):** Real-time messaging, file uploads, and response time calculations.
5.  **Admin Management:** User profiles, Master Data (OPD, Jenis Bencana), and audit logs.

### B. Non-Functional
*   **Security:** Role-based access control (RBAC) audit.
*   **Compatibility:** Cross-browser (Chrome, Firefox, Safari) and Mobile responsiveness.

## 3. Test Strategy & Tools
*   **Methodology:** Automated E2E Regression Testing.
*   **Primary Tool:** **Playwright** (Recommended for its superior handling of IFrames, Maps, and parallel execution).
*   **Language:** TypeScript (to match the "Full JS" potential stack).
*   **Environment:** Current Production (Read-only for baseline) -> New Staging/Dev.

## 4. High-Level Test Scenarios (The "Critical Path")

### Scenario 1: Disaster Reporting Workflow (E2E)
*   **Pre-condition:** User logged in as Regional BPBD.
*   **Actions:**
    1.  Enter 'BARATA CHAT'.
    2.  Submit a new disaster report with an image attachment.
    3.  Verify the "Response Time" timer starts.
    4.  Log in as Provincial Pusdalops.
    5.  Validate the report and update status to "Tanggap Darurat".
*   **Expected Result:** Data is visible in the public 'Peta Kejadian' and 'Infografis' immediately.

### Scenario 2: Map & Spatial Validation
*   **Actions:**
    1.  Navigate to 'Peta Rawan Bencana'.
    2.  Toggle 'Banjir' layer.
    3.  Zoom into a specific region (e.g., Kabupaten Bogor).
    4.  Click a disaster marker.
*   **Expected Result:** Popup shows correct metadata matching the database record.

### Scenario 3: Form Validation & Security
*   **Actions:**
    1.  Attempt to 'Simpan' a new event without mandatory 'OPD' or 'Lattitude'.
    2.  Attempt to access `/user/profil` without an active session.
*   **Expected Result:** System shows specific error messages for empty fields and redirects unauthorized users to login.

## 5. Data Migration Testing Strategy
Before the final migration, we will perform a **Data Reconciliation Test**:
1.  **Count Check:** Ensure `Total Records (Old) == Total Records (New)`.
2.  **Sample Validation:** Select 50 random records (old vs. new) and compare every field (including coordinates and rich text chronology).
3.  **Integrity Check:** Ensure foreign keys (e.g., Link between Disaster Event and its Chat History) are intact.

## 6. Deliverables
*   **E2E Test Suite Code:** Playwright scripts hosted in the repository.
*   **Baseline Report:** PDF/HTML report showing current system pass/fail and performance stats.
*   **Bug Discovery Log:** Any issues found in the *current* system that should be fixed in the *new* one.
