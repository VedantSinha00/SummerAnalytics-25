I was having some problems with the interface of collab and thus has to find some workarounds to complete this project.. !
Please note that I am working on this project on a Windows computer, and it might just be some issue due to my system. 

1. I have put a code block for uploading the database file since the original way that was done in the code wasn't working for me. 
  I have put the database file in the github repo as well, feel free to download it to use for running the code. 
  the database file is by the name of "dataset.csv" in the repository. 


2. the bokeh plotting feature was working very inconsistantly. I had to run the code 10 times before finding that the graph was coming out in only one of them. 
  I have checked the code throughtly and it seems that the problem is either with the interface, or the system :/
  for the same reason, I have downloaded the plot in form of png files and uploaded it in the repository as well.
  Please refer to that to get a better sense of what the plot looks like in case the code causes the same problem for you as well. :)
  The photos are by the name "SA2025_W5_Capstone_bokeh_plot_model_1" and "SA2025_W5_Capstone_bokeh_plot_model2".

Sorry for the inconvinience cased, I had to use these workarounds to make sure that I could submit the project in time. 

Thank you for being patient and understanding.


now, for the project: 

# 🚗 Dynamic Parking Price Optimization using Stream Processing

This project implements a real-time system to compute dynamic parking prices using **Pathway**, a powerful stream processing engine. It ingests live-like parking lot data (occupancy, queue length, traffic conditions, vehicle types, etc.), processes it in **daily tumbling windows**, and computes pricing based on demand indicators. The output is visualized interactively using **Panel** and **Bokeh**.

> 💡 This solution simulates a smart-city component that could optimize parking availability and pricing dynamically across urban areas.

---

## 🧰 Tech Stack

- **Python 3.11+**
- **Pathway** – Stream processing engine
- **Pandas** – Data cleaning & CSV preparation
- **Bokeh** – Interactive plotting
- **Panel** – Web dashboard & UI
- **Google Colab** (for development, though limited for real-time UI)

---

## 🏗 Architecture Diagram

```mermaid
graph TD
    A[CSV Dataset] -->|Replay as stream| B(Pathway Stream Input)
    B --> C[Add datetime columns]
    C --> D[Windowing by Day]
    D --> E[Reduce (aggregations)]
    E --> F[Dynamic Pricing Formulae]
    F --> G[Stream output (t, price, price_model2)]
    G --> H[Visualization with Panel + Bokeh]
```

---

## ⚙️ Project Architecture & Workflow

### 🔹 1. **Data Preparation**
- Input CSV with timestamps, occupancy, capacity, traffic, etc.
- Categorical columns like `TrafficConditionNearby` and `VehicleType` are mapped to numerical values (`0.0`, `0.5`, `1.0`, etc.).
- Final cleaned data is exported as `parking_stream.csv`.

### 🔹 2. **Schema Definition**
- A `ParkingSchema` is created using `pw.Schema`, ensuring all columns are typed (including timestamps, floats, and integers).

### 🔹 3. **Stream Ingestion**
- Pathway's `replay_csv()` simulates a live data stream using the CSV file.
- Additional datetime columns (`t`, `day`) are derived from the timestamp for time-based windowing.

### 🔹 4. **Windowing & Aggregation**
- A **daily tumbling window** is defined using `windowby(...)`.
- Inside each window, metrics are computed:
  - `occ_max`, `occ_min`, `queue_mean`, `vehicle_mean`, etc.

### 🔹 5. **Dynamic Pricing Models**
Two pricing strategies are implemented:
- **Model 1 (Basic):**  
  `price = 10 + (occ_max - occ_min) / capacity`
- **Model 2 (Demand-based):**  
  Normalized demand is computed as a weighted average of occupancy, queue length, traffic, special day flag, and vehicle type.  
  Then:  
  `price_model2 = 10 * (1 + 0.5 * normalized_demand)`

### 🔹 6. **Visualization**
- Two real-time line charts (Model 1 and Model 2 prices) are rendered using Bokeh.
- Panel is used to display the dashboards.
- Streamed data can also be debugged using `.debug()` for transparent inspection.

---

## 📈 Example Output Fields

| Column          | Description                          |
|-----------------|--------------------------------------|
| `t`             | End of time window (per day)         |
| `price`         | Basic dynamic price                  |
| `price_model2`  | Demand-based enhanced pricing        |
| `occ_max`       | Max occupancy observed that day      |
| `queue_mean`    | Average queue length                 |
| `vehicle_mean`  | Mean vehicle type weight             |


---

## 📂 File Structure (Key Files)

```
├── dataset.csv                # will be useful in running the code (as said in the starting) 
├── SA2025_W5_Capstone_project.ipynb                 # Full Colab notebook
└── README for Capstone project.md                  # You're here!
```

---

## ✅ How to Run

1. Install dependencies: `pip install pathway panel bokeh pandas`
2. Prepare your data: run preprocessing block and save `parking_stream.csv`
3. Run the notebook
4. Use `pw.run()` to execute the stream

---

## 👨‍💻 Author

Made by Vedant Sinha, as part of a stream processing course project.
