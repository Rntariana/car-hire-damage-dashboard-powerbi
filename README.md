# car-hire-damage-dashboard-powerbi

# 🚗 WL Car Hire – Fleet Damage Dashboard (Power BI)

An interactive Power BI report that shows **where on the car** scratches and scrapes happen across a car-hire fleet. Damage counts are drawn directly onto an SVG car diagram using the **Synoptic Panel** custom visual, with slicers to filter by make, model and branch.

> **Module:** DBR261 – Belgium Campus  
> **Assessment:** Portfolio of Evidence (POE) – Task 2  
> **Tool:** Power BI Desktop  

(<img width="1917" height="1018" alt="image" src="https://github.com/user-attachments/assets/11ed806a-309c-427c-85c7-717db586c4d9" />
)

---

## 📌 Contents
1. [Overview](#-overview)
2. [Dataset](#-dataset)
3. [Repository structure](#-repository-structure)
4. [How to open the report](#-how-to-open-the-report)
5. [Step-by-step build guide](#-step-by-step-build-guide)
6. [Key insights](#-key-insights)
7. [Troubleshooting & lessons learned](#-troubleshooting--lessons-learned)
8. [Skills demonstrated](#-skills-demonstrated)

---

## 🎯 Overview

WL Car Hire wants to know which parts of its vehicles are damaged most often, and whether this differs by make, model or branch.

This project:
- Imports two Excel sheets and cleans them in **Power Query**
- Creates a calculated **Make and Model** column
- Uses a **one-to-many relationship** between vehicles and damage records
- Visualises damage on an SVG car using the **Synoptic Panel**
- Adds a KPI card, bar chart, column chart and slicers for interactive filtering

---

## 📊 Dataset

File: `data/CarDamage.xlsx`

| Sheet | Rows | Columns | Description |
|-------|------|---------|-------------|
| `Damage` | 388 | `Date`, `Vehicle ID`, `Damage` | One row per damage incident (Jan–Oct 2016) |
| `Vehicles` | 205 | `Vehicle ID`, `Make`, `Model`, `Branch` | One row per vehicle |

**Relationship:** `Vehicles[Vehicle ID]` (one) → `Damage[Vehicle ID]` (many), single cross-filter direction.

- **Makes:** Peugot, Ford, Volkswagen, Vauxhall (9 models)
- **Branches:** Heathrow, Hammersmith, Harrow, Ealing, Brentford, Perivale

---

## 📁 Repository structure

```
car-hire-damage-dashboard-powerbi/
├── data/
│   └── CarDamage.xlsx
├── assets/
│   ├── Car_damage.svg
│   └── WL Car Hire Logo.png
├── visuals/
│   └── synopticPanelByOKViz.1.5.0.0.pbiviz
├── report/
│   └── CarDamage.pbix
├── images/
│   └── dashboard.png
└── README.md
```

---

## ▶️ How to open the report

1. Install [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (free, Windows).
2. Download or clone this repository.
3. Open `report/CarDamage.pbix`.
4. If the Synoptic Panel visual doesn't load, import `visuals/synopticPanelByOKViz.1.5.0.0.pbiviz` (see Step 8).

---

## 🛠 Step-by-step build guide

> **Before you start:** unzip the project folder. Power BI cannot read files from inside a `.zip`.

### Part 1 – Load and clean the data (Power Query)

**Step 1 – Connect to Excel**
1. Open Power BI Desktop.
2. **Home → Excel workbook** and choose `CarDamage.xlsx`.

**Step 2 – Select both sheets**
1. In the Navigator, tick **Damage** and **Vehicles**.
2. Click **Transform Data** (not *Load*) to open Power Query Editor.

**Step 3 – Review the automatic steps**
Power BI creates `Source → Navigation → Promoted Headers → Changed Type` automatically.

**Step 4 – Change `Model` to Text**
Peugeot models (208, 308, 2008) are numbers, which breaks text joining.
1. Select the **Vehicles** query.
2. Click the type icon (`ABC 123`) on the **Model** column header → **Text**.

**Step 5 – Add the `Make and Model` column**
1. **Add Column → Custom Column**.
2. Name: `Make and Model`
3. Formula:
   ```m
   [Make] & " " & [Model]
   ```
4. Click **OK**. Values such as `Volkswagen Tiguan` should appear with no errors.

**Step 6 – Apply**
**Home → Close & Apply**.

### Part 2 – Check the data model

**Step 7 – Verify the relationship**
1. Open **Model view** (third icon on the left sidebar).
2. Confirm a line joins the two tables on `Vehicle ID`, showing `1` on Vehicles and `*` on Damage.
3. Check: **Many to one (\*:1)**, active, cross-filter direction **Single**.

### Part 3 – Build the report

**Step 8 – Import the Synoptic Panel**
1. Go back to **Report view**.
2. In the Visualizations pane click **⋯ → Get more visuals → Import a visual from a file**.
3. Select `synopticPanelByOKViz.1.5.0.0.pbiviz`.
4. A new stacked-layers icon appears below the **⋯**.

**Step 9 – Add the visual and set its fields**
1. Click a blank part of the canvas, then click the Synoptic Panel icon.
2. Drag **Damage → Damage** into **Category**.
3. Drag **Damage → Vehicle ID** into **Measure** and set it to **Count**.

**Step 10 – Load the car graphic**
1. On the visual, click **Local maps**.
2. Select `Car_damage.svg`.
3. The car appears with damaged areas coloured.

> Each shape in the SVG has an ID like `Front_L_Door`. The Synoptic Panel matches these to the values in the `Damage` column.

**Step 11 – Show the numbers on the car**
1. Select the visual → **Format visual** (paintbrush).
2. Turn on **Data labels** and expand it.
3. Set **Display** to **Data value** and increase the text size.

**Step 12 – Add slicers**
1. Click a **blank area** of the page (so no visual is selected).
2. Click the **Slicer** icon.
3. Tick **Vehicles → Make**.
4. Repeat for **Model** and **Branch**.
5. For each slicer: **Format → Slicer settings / Items → text size 12**.

**Step 13 – Add the supporting visuals**
For each one: click a blank area first, then **Home → New visual**.

| Visual | Fields | Shows |
|--------|--------|-------|
| **Card** | `Damage[Vehicle ID]` → Count | Total incidents (388) |
| **Clustered bar chart** | Y-axis: `Vehicles[Branch]`, X-axis: Count of `Vehicle ID` | Damage by branch |
| **Clustered column chart** | X-axis: `Vehicles[Make and Model]`, Y-axis: Count of `Vehicle ID` | Damage by vehicle type |

**Step 14 – Branding and layout**
1. **Insert → Image** → `WL Car Hire Logo.png`.
2. **Insert → Text box** → `WL Car Hire – Fleet Damage Report`.
3. Pick a colour theme: **View → Themes**.
4. Align visuals: select several with **Ctrl + click**, then **Format → Align**.

**Step 15 – Save**
**File → Save as** → `CarDamage.pbix`.

---

## 🔍 Key insights

- **388 damage incidents** across **205 vehicles** (Jan–Oct 2016).
- **Bumpers are the most damaged area:** Front L (37), Rear L (36), Rear R (32), Front R (31).
- **Heathrow (78)** and **Hammersmith (75)** record the most damage; **Perivale (47)** the least.
- **Ford Fiesta (71 incidents)** is the most damaged model, followed by Peugot 208 (61) and Volkswagen Tiguan (51).
- **Peugot** vehicles account for the most incidents by make (134), followed by Ford (116).

> Note: totals by branch and model also reflect fleet size. A damage-per-vehicle measure would give a fairer comparison – a good next step.

---

## 🧰 Troubleshooting & lessons learned

| Problem | Fix |
|---------|-----|
| Error in `Make and Model` column | Change `Model` to **Text** before adding the custom column. |
| New visual replaced my existing one | Click a **blank area** of the page before choosing a new visual type, or use **Home → New visual**. |
| Numbers on the car changed unexpectedly | A slicer or chart bar is selected. Click it again to clear the filter. |
| Can't select `Car_damage.svg` | The project folder is still zipped. **Extract All** first. |
| Synoptic Panel icon missing | Re-import the `.pbiviz` file. |
| Yellow "pending changes" banner | **Close & Apply** in Power Query. |
| Car shapes not coloured | Check that `Damage` values match the SVG IDs (underscores = spaces). |

---

## 🎓 Skills demonstrated

- Importing Excel data into Power BI
- Data cleaning and transformation in Power Query
- Custom columns using M
- Data modelling with one-to-many relationships
- Custom visuals (Synoptic Panel with SVG)
- Interactive dashboards with slicers and cross-filtering
- Dashboard design and layout

---

## 👤 Author

RORISANG NGOEPE – Belgium Campus  
[GitHub](https://github.com/Rntariana)
