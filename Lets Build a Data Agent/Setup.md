# Demo Setup

The demo is sales data for a beach shop. There are 4 tables in a lakehouse and semantic models build from the data to be the data source for the agents. This repo contains the 4 files to create those tables.

## Populate Lakehouse Tables

::: info Instructions
1. Create a lakehouse
1. Create a new notebook and attach the Lakehouse
1. Copy in the code below
:::

### Import code
```Python
# Fabric Notebook (PySpark + Pandas)
# Reads public CSV URLs via pandas, converts to Spark, writes Delta tables

import pandas as pd
basepath = "https://hatfullofdata.github.io/Sessions/Lets%20Build%20a%20Data%20Agent/Datafiles/"
files = [
    {"url": f"{basepath}dim_date.csv","table": "dim_date"},
    {"url": f"{basepath}dim_product.csv","table": "dim_product"},
    {"url": f"{basepath}dim_weather.csv","table": "dim_weather"},
    {"url": f"{basepath}fact_sales.csv","table": "fact_sales"},

]

for f in files:
    url = f["url"]
    table = f["table"]
    print(f"Loading {url} -> {table}")

    # 1) Download CSV with pandas
    pdf = pd.read_csv(url)

    # 2) Optional cleanup of column names
    pdf.columns = (
        pdf.columns.str.strip()
                   .str.replace(" ", "_", regex=False)
                   .str.replace(r"[^A-Za-z0-9_]", "", regex=True)
    )

    # 3) Convert to Spark DataFrame
    sdf = spark.createDataFrame(pdf)

    # 4) Write to default lakehouse as Delta table
    (sdf.write
        .format("delta")
        .mode("overwrite")
        .saveAsTable(table))

    print(f"Done: {table} ({sdf.count()} rows)")

print("All tables loaded.")
```

## Create Semantic Model

