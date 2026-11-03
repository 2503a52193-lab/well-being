# Project 48: Digital Well-being Analytics Dashboard & Universal Student Analytics Platform
## University Data Analytics and Visualization (DAV) Semester Project

**Title:** Digital Well-being Analytics Dashboard  
**Research Question:** *Does screen time influence students' academic performance?*  
**Final Output:** Interactive, High-Fidelity Full-Stack Web Application with Universal Dataset Ingestion  
**Target Environment:** Windows 11 (Standard Local Setup)

---

### 1. Executive Summary & Research Findings

This application investigates whether daily social media consumption and screen exposure before sleep correlate with students' academic performance across diverse cohorts.

By default, the platform operates on the verified sample dataset:
`Teen_Mental_Health_Dataset.csv(6).xlsx` (1,200 student records, 13 features).

#### Core Empirical Findings (Sample Cohort):
- **Daily Social Media Hours vs. GPA:** $r = +0.013$ (negligible linear association; heavier usage does not linearly correlate with lower GPA across the cohort).
- **Pre-Sleep Screen Time vs. GPA:** $r = -0.035$ (very slight negative tendency, but within statistical noise $|r| < 0.05$).
- **Correlation $\neq$ Causation:** The dashboard emphasizes scientific rigor: absence of linear correlation does not rule out complex non-linear effects, nor does it establish causation. Confounding factors such as unrecorded study hours, homework completion, parental supervision, and student motivation play critical roles.
- **Dataset Constraints:** The original survey dataset does not contain a `study_hours` variable. The dashboard explicitly annotates this limitation in the Academic Analytics module and unlocks dedicated Study Hours analytics when custom datasets providing study hours are uploaded.
- **Ethical Integrity:** Analytics are displayed strictly at population and cohort levels. No individual student is labeled or diagnosed.

---

### 2. Universal Student Dataset Upload & Analysis ("Upload & Analyze")

The dashboard is a **reusable analytics platform**. Teachers and researchers can upload any student dataset in Excel (`.xlsx`, `.xls`) or CSV (`.csv`) format with differing column names and scoring structures.

#### Platform Features:
1. **Intelligent Synonym Auto-Detection:** Automatically matches arbitrary column names to canonical analytics variables:
   - *Screen Time / Social Media:* `screen_time`, `phone_usage`, `mobile_hours`, `screen_hours`, `daily_social_media_hours`, `media_time`
   - *Academic Achievement:* `GPA`, `cgpa`, `marks`, `exam_score`, `final_score`, `academic_performance`, `grade_point_average`
   - *Sleep Duration:* `sleep_duration`, `sleep_hours`, `nightly_sleep`, `hours_sleep`, `rest_hours`
   - *Study Duration:* `study_time`, `study_hours`, `study_duration`, `prep_time`, `independent_study`
   - *Physical Activity:* `workout_hours`, `exercise_hours`, `physical_activity`, `sports_hours`
   - *Mental Well-being:* `stress`, `stress_level`, `anxiety`, `anxiety_level`, `depression_label`
2. **Interactive Column-Mapping Matrix:** Teachers can review auto-detected matches, override mappings via clear dropdowns, or mark variables as optional.
3. **Dynamic Academic Scoring Scales:** Supports 5 academic scoring systems:
   - **4.00 GPA Scale** (standard secondary/college GPA)
   - **10.0 CGPA Scale** (common university CGPA)
   - **100% Percentage / Marks** (0 - 100% scale)
   - **50 Marks Scale** (half-semester tests)
   - **Custom Scoring Scale** (teacher-specified min, max, and unit)
4. **Study Hours Analytics:** If the uploaded dataset contains study duration, the dashboard activates the *Academic Performance vs. Study Hours* module, displaying scatter plots, performance tier correlations, and KPI metrics.
5. **Missing Variable Protection:** If optional indicators (e.g. platform, stress, physical activity) are missing in the uploaded dataset, the dashboard gracefully disables corresponding charts with clear informative notices without throwing errors.
6. **Dataset Switching:** Teachers can toggle anytime between the "Original Sample Dataset (1,200 Students)" and the "Uploaded Custom Dataset" from the top navigation bar.
7. **Privacy & Local Session Storage:** Uploaded student files are processed strictly in-memory and in temporary local session storage. Records are never sent to external AI APIs or third-party cloud servers. A 1-click **Delete Uploaded Data** button removes uploaded files and resets the platform.

---

### 3. Technology Stack

- **Backend:**
  - Language: Python 3.10+ (tested on Python 3.14)
  - Web Framework: Flask 3.0+ & Flask-CORS
  - Data Processing: Pandas, NumPy, SciPy
  - Excel Reader: `openpyxl`
- **Frontend:**
  - Framework: React 18 (Vite build tool)
  - Styling: Tailwind CSS & Lucide React icons
  - Charting Engine: Recharts (Histograms, Scatter Plots, Multi-axis Bar Charts, Area & Pie Charts)
- **Local Proxy & API:**
  - Seamless Vite proxy routing `/api` calls to Flask on `127.0.0.1:5000`.

---

### 4. Application Structure

```
well-being-dashboard/
├── Teen_Mental_Health_Dataset.csv(6).xlsx   # Original single source of truth (1,200 records)
├── backend/
│   ├── app.py                               # Flask REST API endpoints + CORS
│   ├── data_service.py                      # Data analysis service (openpyxl + Pandas)
│   ├── dataset_manager.py                   # Dataset upload, schema inspection & mapping engine
│   ├── uploads/                             # Session-isolated temporary dataset storage
│   ├── requirements.txt                     # Python dependencies
│   └── tests/
│       ├── test_api.py                      # Regression & JSON RFC 8259 compliance test suite
│       └── test_upload_and_mapping.py       # Universal upload, synonym & scale test suite
├── frontend/
│   ├── index.html                           # HTML5 entry with Plus Jakarta Sans
│   ├── package.json                         # Node dependencies & Vite scripts
│   ├── vite.config.js                       # Vite proxy config
│   ├── tailwind.config.js                   # Tailwind theme extension
│   ├── postcss.config.js                    # PostCSS plugins
│   └── src/
│       ├── main.jsx                         # React root mount
│       ├── App.jsx                          # Main shell with active dataset state & routing
│       ├── index.css                        # Tailwind directives
│       ├── components/
│       │   ├── Sidebar.jsx                  # Module navigation & active dataset card
│       │   ├── Header.jsx                   # University project banner & dataset switch pill
│       │   ├── FilterBar.jsx                # Dynamic Age, Gender, Platform & Usage sliders
│       │   ├── MetricCard.jsx               # Headline KPI cards with tooltips
│       │   ├── LoadingSpinner.jsx           # Animated loading state
│       │   └── ErrorAlert.jsx               # Graceful error alerts & retry handling
│       ├── pages/
│       │   ├── UploadPage.jsx               # Universal Dataset Upload, Inspection & Mapping
│       │   ├── OverviewPage.jsx             # Core KPIs + Dynamic Scales + Platform breakdown
│       │   ├── SocialMediaPage.jsx          # Usage distribution, age-wise & scatter plot
│       │   ├── AcademicPage.jsx             # Performance tiers, study hours analytics & sleep
│       │   ├── WellBeingPage.jsx            # Stress/anxiety scales, exercise & sleep patterns
│       │   ├── InsightsPage.jsx             # Pearson correlation matrix & research synthesis
│       │   └── ReportsPage.jsx              # Summary report & Aggregate CSV export
│       └── services/
│           └── api.js                       # REST API client with upload & dataset methods
└── README.md                                # Project documentation
```

---

### 5. Installation & Execution Instructions (Windows 11)

#### Prerequisites:
1. **Python 3.10+** (with `pip` added to PATH)
2. **Node.js v18+** & **npm**

---

#### Step 1: Start the Backend (Flask API)

Open a PowerShell terminal in the project root:

```powershell
# Navigate to backend directory
cd C:\Users\2503a\OneDrive\Desktop\well-being-dashboard\backend

# Install required Python packages:
pip install -r requirements.txt

# Run the Flask backend server:
python app.py
```

The backend server will start at: `http://127.0.0.1:5000`  
Health check endpoint: `http://127.0.0.1:5000/api/health`

---

#### Step 2: Start the Frontend (Vite + React)

Open a second PowerShell terminal in the project root:

```powershell
# Navigate to frontend directory
cd C:\Users\2503a\OneDrive\Desktop\well-being-dashboard\frontend

# Install node dependencies:
npm install

# Launch Vite development server:
npm run dev
```

Open your browser to: **`http://localhost:5173`**

---

### 6. Running Automated Backend & Regression Tests

To run the full automated `pytest` verification suite (25 tests covering both core analytics and universal uploads):

```powershell
cd C:\Users\2503a\OneDrive\Desktop\well-being-dashboard
python -m pytest backend/tests -v
```

The 25 test suites verify:
- Health check & original Excel dataset loading (1,200 records).
- Strict RFC 8259 JSON compliance with zero raw `NaN` or `Infinity` tokens.
- Dynamic filter bounds (Gender, Platform, Age span, Max Social Media Hours).
- Filter thresholds across 8.0h, 7.0h, 6.0h, 5.5h, 5.0h, 4.0h, 3.0h, 2.0h, and 1.0h.
- Empty cohort handling (converts undefined statistical results to JSON `null`, displaying "Not available").
- CSV and Excel file schema inspection.
- Token-set synonym matching for column aliases (e.g. `exam_score` $\to$ `academic_performance`, `phone_usage` $\to$ `daily_social_media_hours`, `Hours_Sleep` $\to$ `sleep_hours`, `study_time` $\to$ `study_hours`).
- Scoring scale auto-detection (4.0 GPA, 10.0 CGPA, 100% Marks, Custom).
- Activation of Study Hours analytics when mapped.
- Graceful missing metric fallbacks when optional columns are absent.
- Dataset switching and session deletion.
- Anonymized aggregate CSV export.

---

### 7. Interactive Modules Overview

1. **Upload & Analyze:** Universal spreadsheet ingestion wizard supporting `.xlsx`, `.xls`, and `.csv`. Auto-detects columns, offers review dropdowns, configures academic scoring scales, and provides 5-row raw previews.
2. **Dashboard Overview:** Displays total records, average social media hours, average sleep hours, academic score on the calibrated scale, pre-bed screen time, and study hours (if present).
3. **Digital Habits:** Visualizes daily usage distributions, comparative platform statistics, age-stratified usage, and social media vs. academic achievement scatter plots.
4. **Academic Analytics:** Visualizes academic performance distributions across custom scale brackets, score vs. sleep duration, score vs. pre-sleep screen time, and score vs. independent study hours.
5. **Digital Well-being:** Detailed distributions of perceived stress (1-10), anxiety (1-10), device addiction ratings, physical activity by gender, and sleep pattern correlations.
6. **Data Insights:** Presents an interactive Pearson correlation matrix, descriptive statistics table generated via Pandas, and an empirical answer to the research question with dataset limitations.
7. **Reports & Export:** Allows instant download of aggregated, anonymized cohort analytics as CSV and provides a clean, printable executive summary report.


