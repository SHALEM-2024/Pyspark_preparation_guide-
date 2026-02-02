# Pyspark_preparation_guide
This repository guides an individual preparing pyspark for interviews. 

Overview:

# PySpark Essential Functions Cheatsheet

This guide outlines the top, most essential functions for day-to-day PySpark development, categorized by their role in the data manipulation process.

## 1. DataFrame Methods (`df.`)
**Type:** Structural "Verbs"  
**Role:** Used to shape the table structure. These methods are called directly on your DataFrame object.

| Sub-Category | Function | Description |
| :--- | :--- | :--- |
| **Selection & Structure** | `select()` | Choose specific columns to keep. |
| | `withColumn()` | Create a new column or update an existing one. |
| | `withColumnRenamed()` | Rename an existing column. |
| | `drop()` | Remove a column from the DataFrame. |
| | `printSchema()` | Print the column names and data types (debugging). |
| **Filtering & Sorting** | `filter()` / `where()` | Filter rows based on a condition (SQL style or logic). |
| | `orderBy()` / `sort()` | Sort rows by specific columns. |
| | `distinct()` | Remove duplicate rows. |
| | `limit()` | Restrict the output to the top N rows. |
| **Grouping & Joining** | `groupBy()` | Group data (followed by an aggregate function). |
| | `join()` | Merge two DataFrames based on a key. |
| | `union()` / `unionByName()` | Append rows from one DataFrame to another. |
| **Actions (Triggers)** | `show()` / `display()` | View the data (triggers computation). |
| | `count()` | Return the number of rows. |
| | `write` | Save data (e.g., `df.write.parquet(...)`). |

---

## 2. Column Functions (`F.`)
**Type:** Logic "Adjectives"  
**Role:** Used inside DataFrame methods to define calculation logic.  
**Import:** `from pyspark.sql import functions as F`

| Sub-Category | Function | Description |
| :--- | :--- | :--- |
| **Basics** | `col()` | Refers to a column (essential for complex logic). |
| | `lit()` | Creates a literal (constant) value. |
| | `expr()` | Allows you to write raw SQL snippets as code. |
| **Conditional** | `when().otherwise()` | The `if-then-else` logic of Spark. |
| | `coalesce()` | Returns the first non-null value in a list of columns. |
| **Aggregation** | `sum()`, `avg()`, `mean()` | Basic math statistics. |
| | `min()`, `max()` | Finds boundaries of data. |
| | `count()`, `countDistinct()` | Counts rows or unique values. |
| **String Ops** | `concat()`, `concat_ws()` | Joins strings together. |
| | `substring()` | Extracts part of a string. |
| | `upper()`, `lower()`, `trim()` | Formatting text. |
| | `split()` | Splits a string into an array. |
| **Date & Time** | `current_date()` | Gets today's date. |
| | `to_date()`, `to_timestamp()` | Converts strings to date/time objects. |
| | `date_add()`, `datediff()` | Adds days or finds difference between dates. |
| **Window-Specific** | `row_number()` | Assigns a unique ID to rows in a window. |
| | `rank()`, `dense_rank()` | Ranking (dense = no gaps in rank numbers). |
| | `lead()`, `lag()` | Gets value from next row or previous row. |
| **Complex Types** | `explode()` | Explodes an array column into multiple rows (one per item). |
| | `struct()` | Combines columns into a nested object. |

---

## 3. Window Specifications (`Window.`)
**Type:** Scope Definitions  
**Role:** Defines the context or "frame" for calculations.  
**Import:** `from pyspark.sql import Window`

*Note: This category is small because it only has one job—defining the "frame."*

| Function | Description |
| :--- | :--- |
| `partitionBy()` | **The Grouper.** Splits the data into logical groups (like `groupBy`, but keeps rows). |
| `orderBy()` | **The Sorter.** Determines the order of rows *within* the partition (crucial for ranking). |
| `rowsBetween()` | **The Slider (Physical).** Looks at physical rows relative to current (e.g., "previous 1 row"). |
| `rangeBetween()` | **The Slider (Logical).** Looks at value ranges relative to current (e.g., "dates within 7 days"). |
| `unboundedPreceding` | **Boundary.** Start at the very first row of the partition. |
| `unboundedFollowing` | **Boundary.** End at the very last row of the partition. |

---

## Top 3 "Power Combos"

Here is how developers combine these three categories in real scripts:

### 1. The "Feature Engineer"
> **Combo:** `df.withColumn()` + `F.when()` + `F.col()`

**Use Case:** Used to create new flags or categories based on logic.

### 2. The "Deduplicator"
> **Combo:** `Window.partitionBy()` + `F.row_number()` + `df.filter()`

**Use Case:** Used to keep only the "most recent" record per user.

### 3. The "Time Traveler"
> **Combo:** `Window.partitionBy()` + `F.lag()` + `df.withColumn()`

**Use Case:** Used to compare today's data with yesterday's data.

then moves on to practical questions then deep dives into pyspark performance optimization (Data Layout Optimization) through partitionBy, Bucketing and Salting, Z-Ordering, then to building spark clusters.
