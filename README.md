# Bash_path

# README - SQL File Path Insertion

## Overview
This script automates the insertion of file path information into two Delta tables in Databricks:
1. **base_path_info** – Stores the base path directory information.
2. **latest_partition_path_info** – Stores file names and their full paths under the base path.

It ensures **only new records are appended** without duplicating existing entries.

---

## How It Works
1. **Configuration:**
   - Set `base_path` to your directory location in Databricks.
2. **Recursive File Collection:**
   - Uses `dbutils.fs.ls` to recursively list all files under the base path.
3. **Insert Base Path Info:**
   - Adds an entry for the base path in `base_path_info` table **only if it does not already exist**.
4. **Insert File Paths:**
   - Adds new file records in `latest_partition_path_info` table **only if they do not already exist**.
5. **Duplicate Prevention:**
   - `WHERE NOT EXISTS` SQL condition ensures no duplicate records.

---

## Tables Used
- **my_unity_catalog_metastore.base_path.base_path_info**
  - Columns: `base_path_name`, `path_location`
- **my_unity_catalog_metastore.base_path.latest_partition_path_info**
  - Columns: `large_path_Name`, `latest_path_location`

---

## Running the Script
1. Place this script in a Databricks notebook.
2. Modify the `base_path` variable to point to your desired directory.
3. Run the notebook.

The script will:
- Insert the base path info if new.
- Insert all file paths under that base path if new.

---

## Example
**Base Path:**
```
/Volumes/workspace/default/my_file/bash_path/
```
**Inserted Records:**
- `base_path_info` → `my_file`, `/Volumes/workspace/default/my_file/bash_path/`
- `latest_partition_path_info` → `hospital_10_days_data.csv`, `dbfs:/Volumes/.../hospital_10_days_data.csv`

---

## Notes
- No deletion is performed.
- Safe to re-run; only new data will be appended.
