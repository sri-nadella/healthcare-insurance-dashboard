# US Healthcare Insurance Cost Dashboard — Power BI (Mid-Level)

A 3-page interactive Power BI dashboard analyzing healthcare insurance costs across 1,338 patients. Built with professional DAX measures, cross-filtering slicers, drill-through pages, and a What-If BMI threshold parameter for dynamic risk simulation.

---

## Dashboard Preview

### Page 1 — Insurance Cost Overview
![Page 1](page1_overview.png)

### Page 2 — Regional Deep Dive
![Page 2](page2_regional.png)

### Page 3 — Risk Analysis
![Page 3](page3_risk.png)

---

## Dataset

- **Source:** Kaggle — US Health Insurance Dataset
- **Records:** 1,338 patients
- **Fields:** Age, Sex, BMI, Children, Smoker status, Region, Insurance charges

---

## Dashboard Structure

### Page 1 — Insurance Cost Overview
Five KPI cards + four charts giving a complete overview of cost drivers.

**KPI Cards:**
| Metric | Value |
|---|---|
| Average Insurance Cost | $13,270 |
| Total Patients | 1,338 |
| Highest Insurance Cost | $63,770 |
| Smoker Cost Multiplier | 3.80x |
| High Risk Patients | 145 |

**Charts:**
- Average cost by region (bar chart)
- Smokers vs non-smokers cost split (donut chart)
- Age vs charges by smoker status (scatter plot)
- Average cost by BMI category (column chart)

---

### Page 2 — Regional Deep Dive
Interactive page with cross-filtering slicers and regional breakdown.

**Slicers:** Region (dropdown), Smoker status (checkbox)

**Visuals:**
- Avg cost by region and gender
- Patient count by region
- High risk patients by region
- Regional summary table (all 4 measures per region)

**Key Finding:** Southeast has the highest average cost ($14,735), most high risk patients (58), and highest smoker premium (4.34x).

---

### Page 3 — Risk Analysis
Dynamic risk simulation using a What-If BMI threshold parameter.

**What-If Parameter:** BMI Threshold slider (18–45, default 30)
- Moving the slider dynamically recalculates high risk patient count
- At BMI ≥ 30: 145 high risk patients
- At BMI ≥ 37: 39 high risk patients

**Visuals:**
- Dynamic high risk patients card (updates with slider)
- Avg cost by BMI category and smoker status
- Dynamic high risk patients by region
- BMI vs insurance charges scatter plot

---

## DAX Measures

All measures stored in a dedicated `Measures` table:

```dax
Avg Cost = AVERAGE(insurance[charges])

Total Patients = COUNTROWS(insurance)

Max Cost = MAX(insurance[charges])

Smoker Avg Cost = 
CALCULATE(
    AVERAGE(insurance[charges]),
    insurance[smoker] = "yes"
)

Non Smoker Avg Cost = 
CALCULATE(
    AVERAGE(insurance[charges]),
    insurance[smoker] = "no"
)

Smoker Premium = 
DIVIDE(
    [Smoker Avg Cost],
    [Non Smoker Avg Cost],
    0
)

High Risk Patients = 
CALCULATE(
    COUNTROWS(insurance),
    insurance[smoker] = "yes",
    insurance[bmi] >= 30
)

Dynamic High Risk = 
VAR SelectedBMI = MAX('BMI Threshold'[BMI Threshold])
RETURN
CALCULATE(
    COUNTROWS(insurance),
    insurance[smoker] = "yes",
    insurance[bmi] >= SelectedBMI
)
```

## DAX Calculated Column

```dax
BMI Category = 
IF(insurance[bmi] < 18.5, "Underweight",
IF(insurance[bmi] < 25, "Normal",
IF(insurance[bmi] < 30, "Overweight", "Obese")))
```

---

## Key Insights

- **Smoker status is the #1 cost driver** — smokers pay 3.80x more than non-smokers
- **Southeast region** has the highest average cost ($14,735) and most high risk patients (58)
- **145 patients** are both smokers and obese — the highest risk group representing 10.8% of all patients
- **Age and charges are positively correlated** — two distinct cost bands visible for smokers vs non-smokers
- **Obese patients cost significantly more** across all regions regardless of smoker status
- **Dynamic simulation** shows that tightening the BMI threshold from 30 to 37 reduces high risk count from 145 to 39

---

## Mid-Level Features Used

| Feature | Implementation |
|---|---|
| DAX Measures | 8 measures in dedicated Measures table |
| Calculated Column | BMI Category using nested IF |
| What-If Parameter | BMI Threshold slider (18–45) |
| Cross-filtering | Region + smoker slicers on Page 2 |
| Multi-page dashboard | 3 pages with different analytical focus |
| Summary table | Regional breakdown with all measures |
| Dynamic visuals | Charts update based on slicer selection |

---

## Tech Stack

```
Tool:     Microsoft Power BI Desktop
Dataset:  US Health Insurance (Kaggle, CSV)
DAX:      8 measures + 1 calculated column + What-If parameter
Pages:    3 (Overview, Regional Deep Dive, Risk Analysis)
```

---

## Project Structure

```
healthcare-insurance-dashboard/
├── healthcare_insurance_dashboard_v2.pbix  # Updated Power BI file
├── insurance.csv                            # Raw dataset
├── page1_overview.png                       # Page 1 screenshot
├── page2_regional.png                       # Page 2 screenshot
├── page3_risk.png                           # Page 3 screenshot
└── README.md
```

---

## How to Run

1. Install Power BI Desktop free from `https://powerbi.microsoft.com/desktop`
2. Clone this repo
3. Open `healthcare_insurance_dashboard_v2.pbix`
4. All 3 pages load automatically with full interactivity
