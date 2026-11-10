# 🏛️ IIIT Bhubaneswar Mess Management & Automated Billing Portal

A dynamic, production-grade, full-stack web application designed for the **Hostels & Central Mess Affairs Committee, International Institute of Information Technology (IIIT), Bhubaneswar**.

The system automates daily meal management, real-time digital meal passes (QR check-in), leave rebates calculation, canteen extras ordering, feedback grievance redressal, and full-featured monthly billing with simulated UPI/Card payments and official printable tax receipts.

---

## 🌟 Key Functional Modules

### 1. 💳 Billing & Payment System (Core Feature)
- **Monthly Mess Invoicing**:
  - Automatically calculates base mess boarding fee (`30 days * ₹125/day = ₹3,750`).
  - Itemized deductions for approved mess leaves (`-₹750 for 6 rebate days`).
  - Itemized additions for canteen & night mess add-ons (`+₹380`).
  - Infrastructure & kitchen maintenance levy (`+₹150`).
  - Live calculation of Net Payable Amount (`₹3,530`).
- **Interactive Multi-channel Payment Gateway**:
  - **UPI / Dynamic QR Code**: Pay via Google Pay, PhonePe, Paytm, BHIM, or manual VPA (`rollno@oksbi`).
  - **Net Banking**: Instant integration with SBI, HDFC, ICICI, PNB, Canara Bank.
  - **Debit / Credit Card**: RuPay, Visa, MasterCard entry with card formatting.
  - Interactive authorization simulator with confetti celebration.
  - Generates verifiable Receipt Number (`RCP-NOVEMBER2026-XXXX`) and Transaction ID (`UPI/TXN-...`).
- **Printable Official Tax Receipt & Invoice**:
  - Institutional crest and header of IIIT Bhubaneswar (Govt. of Odisha autonomous university).
  - Comprehensive particulars: Roll Number, Hostel, Room No, Branch, Allocation.
  - Full itemized ledger breakdown.
  - Digital verification QR seal and Chief Warden authorized signature.
  - One-click browser print / PDF download (`Ctrl+P` ready with clean `@media print` styling).

### 2. 🏖️ Mess Leave & Rebate Management
- **Institute Ordinance Enforcement**:
  - Enforces IIIT Bhubaneswar's policy: minimum **3 consecutive days** of absence required for rebate eligibility.
  - Automatic refund rate calculation: `Duration * ₹125/day`.
  - Prevents overlapping leave applications.
- **Workflow & Verification**:
  - Students submit applications with leave category (Academic/SIH Hackathon, Vacation, Medical, Internship) and proof reference.
  - Chief Warden / Mess Committee reviews and clicks **Approve** or **Reject** with formal remarks.
  - On approval, the rebate credit is **immediately deducted** from the student's active monthly bill or added to their mess wallet!

### 3. 🍽️ Live Dynamic Menu & Digital Meal Pass
- **Daily 4-Meal Service**:
  - Breakfast (7:30 – 9:30 AM), Lunch (12:30 – 2:30 PM), Evening Snacks (5:00 – 6:30 PM), Dinner (7:45 – 9:45 PM).
  - Highlights authentic Odia & Pan-Indian delicacies (Authentic Odia Dalma, Machha Besara, Pahala Rasgulla, Chhena Poda, Paneer Butter Masala, Hyderabadi Dum Biryani).
  - Nutritional breakdown (calories & protein) and Chef's Specials.
- **Dynamic Digital QR Meal Pass**:
  - High-resolution QR code pass with student roll number, real-time clock, and meal slot.
  - Prevents double punching and rejects meal access if student is on active approved rebate.

### 4. 📱 Mess Manager / Caterer Counter Scanner & POS
- **Counter Scanner & Check-in**:
  - Scan or enter Roll No to verify eligibility.
  - Automatic double-punch protection.
  - Automatic alert if student is on approved leave (prevents proxy meals).
- **Canteen Extras POS (Point of Sale)**:
  - 1-click addition of extra items (Boiled Eggs, Chicken Roast, Cold Coffee, Kadai Paneer, Maggi) directly to student's monthly ledger.
- **Dynamic Menu Editor**:
  - Update dishes, calories, and specials for any day/slot with live reload.
- **Kitchen Grocery & Inventory**:
  - Track stock levels of Rice, Atta, Dal, Oil, Milk, and LPG cylinders with low-stock warnings.

### 5. 👨‍🏫 Chief Warden & Admin Control Center
- **Executive Analytics**:
  - Visual charts for Billed Revenue vs Collections, Outstanding Dues, and Rebate Deductions.
  - Daily attendance breakdown across Breakfast, Lunch, Snacks, and Dinner.
  - Meal satisfaction score tracker (★ 4.2 / 5.0).
- **1-Click Billing Cycle Generator**:
  - Issue new invoices across all hostels (Ramanujan BH-1, Visvesvaraya BH-2, Kalpana Chawla GH) with customized per-day rate and maintenance levy.
- **Grievance Redressal Desk**:
  - Address student complaints, issue directives to caterer, and record formal resolution notes.

---

## 👥 Demo Personas (Switchable via UI)

You can toggle between different roles in 1 click using the top-right persona menu:

| Role | Name | Roll No / ID | Hostel / Designation | Key Access |
|---|---|---|---|---|
| **Student (Non-Veg)** | Aman Kumar Dash | `B122045` | Ramanujan Hall (BH-1) | Live Menu, QR Pass, Unpaid Bill, Apply Rebate, Extras |
| **Student (Veg)** | Priyanka Mohapatra | `B122018` | Kalpana Chawla Hall (GH) | Paid Bill Receipt, Apply Rebate, Grievance Portal |
| **Student (Non-Veg)** | Rahul Patnaik | `B123089` | Visvesvaraya Hall (BH-2) | Unpaid Bill, Medical Rebate, Menu Viewer |
| **Mess Manager** | Mr. Ramesh Jena | `STAFF-MGR-01` | Central Mess Hall 1 | Counter Scanner, Extras POS, Menu Editor, Inventory |
| **Chief Warden** | Prof. Dr. Suvendu Sahoo | `ADMIN-WDN-01` | Hostel Administration | Rebate Approvals, Invoicing Cycles, Executive Analytics |

---

## 🛠️ Technology Stack

- **Frontend**:
  - React 19 (Hooks, Context, Modular Components)
  - Vite (Ultra-fast HMR and bundle compilation)
  - Tailwind CSS v4 (Modern responsive utility system)
  - Lucide React (Crisp iconography)
  - QRCode.react (Dynamic QR code generation)
  - Canvas Confetti (Celebratory payment animation)
- **Backend**:
  - Node.js & Express.js REST API
  - Zero-dependency JSON Database Engine with atomic persistence (`server/database.json`)
  - Modular API routers: Auth, Billing, Rebate, Menu, Attendance, Extras, Grievance, Inventory, Analytics
- **Deployment**:
  - Unified server serving production frontend bundle at `http://localhost:5000` with API at `/api/*`.

---

## 🚀 How to Run the Application

### 1. Unified Full-Stack Mode (Recommended)
The backend serves both the API and the compiled React frontend:
```powershell
node server/index.js
```
Open **[http://localhost:5000](http://localhost:5000)** in your web browser.

### 2. Development Mode (Hot Reload)
To run frontend and backend independently with Vite hot module replacement:

**Terminal 1 (Backend API):**
```powershell
node server/index.js
```

**Terminal 2 (Frontend Dev Server):**
```powershell
cd client
npm.cmd run dev
```
Open **[http://localhost:5173](http://localhost:5173)** in your browser (Vite automatically proxies `/api` requests to port 5000).

---

## 🧪 Testing Key Workflows

1. **Test Online Bill Payment**:
   - As **Aman Kumar Dash** (`B122045`), go to the **Billing & Payments** tab.
   - Click **Pay Current Dues (₹3,530)**.
   - Choose **UPI**, **Net Banking**, or **Card** and click **Pay Instantly**.
   - Watch the payment authorization and confetti burst!
   - Click **View & Print Official Receipt** to preview the signed IIIT Bhubaneswar receipt.

2. **Test Mess Rebate Application & Warden Approval**:
   - Switch to **Aman Kumar Dash** -> Click **Apply Rebate**.
   - Select a 4-day date range (e.g., Nov 25 to Nov 28) -> see live calculation `4 days = ₹500 credit`.
   - Submit the rebate.
   - Switch role to **Prof. Dr. Suvendu Sahoo (Chief Warden)**.
   - Go to **Rebate Approvals** tab -> click **Approve**.
   - Notice how the rebate deduction is automatically credited to the student's bill!

3. **Test Counter QR Check-in & Proxy Prevention**:
   - Switch to **Mr. Ramesh Jena (Mess Manager)** -> Go to **Counter Check-in Scanner**.
   - Enter `B122045` and select **Lunch** -> Click **Verify**.
   - Notice duplicate punch or rebate detection that enforces mess policy!
