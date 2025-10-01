# Digital Twin Update Pipeline (IFC-Based Construction Monitoring)

This repository provides a complete data pipeline for managing and updating **IFC models** based on **construction scan data** and **scheduled progress**. It supports the extraction, categorization, visualization, analysis, and status update of building components through geometry and property data.

---

## Pipeline Modules

### 1. IFC Info Extraction
**File(s):** `IFC_info_extraction.py`

- Extracts element information from an `.ifc` file:
  - GUID, Task ID, Task Name
  - Scheduled Start/Finish Dates
  - Bounding Box Geometry (from 3D geometry)
- Outputs a `.txt` file: `IFC_info_extraction.txt`

---

### 2. Categorize Elements by Scheduled Date
**File(s):** `categorize_ifc_elements_by_date.py`

- Parses `IFC_info_extraction.txt`
- Categorizes elements by **scan date intervals**
- Saves categorized `.txt` files (1 per interval) in:
  - `categorized_IFC_data/`

---

### 3. Convert Bounding Boxes to OBJ
**File(s):**
- `generate_bounding_box_obj.py`
- `visualize_bounding_boxes.py`

- Reads bounding boxes from `.txt`
- Converts to wireframe `.obj` format for 3D visualization
- Visualizes in notebook (using `matplotlib`)

---

### 4. Count Point Cloud Density (PCD Matching)
**File(s):** `count_points_in_bboxes.py`

- Loads bounding boxes from `.txt` and a `.ply` point cloud file
- For each box, counts how many points are inside
- Saves as: `no_of_points/points_in_bounding_boxes_YYYY-MM-DD.txt`

---

### 5. Extract Completed/Undetected GUIDs
**File(s):**
- `extract_completed_guids.py`
- `extract_undetected_guids.py`

- Filters elements:
  - **Completed** if point count ≥ 200
  - **Undetected** if point count < 200
- Saves GUID lists and filtered bounding boxes
- Output:
  - `output/GUID_detecteddate_YYYY-MM-DD.txt`
  - `undetected_elements/GUID_undetected_YYYY-MM-DD.txt`

---

### 6. Update Construction Status in IFC
**File(s):** `update_ifc_status.py`

- Reads IFC file + GUID status file
- Adds or updates `Pset_ConstructionStatus` for each element:
  - `Status = Completed / Undetected`
  - `UpdatedDate = e.g. 02 May 2024`
- Saves updated IFC model: `construction_status_YYYYMMDD.ifc`

---

### 7. Color Undetected Elements Red
**File(s):** `set_color_red_for_undetected.py`

- Applies red material style to IFC elements with undetected status
- Result: `.ifc` file with red-highlighted elements for visual tracking

---

### 8. Additional Tools

- `split_by_finish_date.py`: splits `IFC_info_extraction.txt` by scheduled finish date
- `reformat_categorized_txt.py`: reformats categorized TXT files with aligned columns
- `sort_unique_dates.py`: extracts and sorts all unique scheduled finish dates

---

## Folder Structure

```text
📁 categorized_IFC_data/
📁 BoundingBox_OBJ_Files/
📁 no_of_points/
📁 undetected_elements/
📁 output/
