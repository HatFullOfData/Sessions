# Demo Setup

As I'm doing this session more than once I'm saving my setup notes here. Mostly for me but feel free to use these to reproduce the demos on your own tenancy.

## Database

Create a Fabric Database and run the following to create three tables

### Calendar Table and Data for 2024 and 2025

``` SQL
CREATE TABLE Calendar (
    Date DATE PRIMARY KEY,
    Date_String VARCHAR(10) NOT NULL
    Year INT NOT NULL,
    MonthNumber INT NOT NULL,
    MonthName VARCHAR(20) NOT NULL
);

WITH DateSequence AS (
    SELECT CAST('2024-01-01' AS DATE) AS Date
    UNION ALL
    SELECT DATEADD(DAY, 1, Date)
    FROM DateSequence
    WHERE Date < '2025-12-31'
)
INSERT INTO Calendar (Date, Date_String, Year, MonthNumber, MonthName)
SELECT 
    Date,
    CONVERT(VARCHAR(10), Date, 23) AS Date_String,
    YEAR(Date) AS Year,
    MONTH(Date) AS MonthNumber,
    DATENAME(MONTH, Date) AS MonthName
FROM DateSequence
OPTION (MAXRECURSION 731);
```

### Sales Table and Data

One row per month, keep it simple

```SQL
CREATE TABLE Sales (
    SalesID INT IDENTITY(1,1) PRIMARY KEY,
    SalesDate DATE NOT NULL,
    SalesAmount DECIMAL(18, 2) NOT NULL
);
INSERT INTO Sales (SalesDate, SalesAmount)
VALUES
    -- 2024 records
    ('2024-01-01', 450.00),    ('2024-02-01', 780.50),
    ('2024-03-01', 325.75),    ('2024-04-01', 890.25),
    ('2024-05-01', 650.00),    ('2024-06-01', 275.50),
    ('2024-07-01', 920.00),    ('2024-08-01', 540.75),
    ('2024-09-01', 185.25),    ('2024-10-01', 710.00),
    ('2024-11-01', 995.50),    ('2024-12-01', 430.00),
    -- 2025 records
    ('2025-01-01', 560.00),    ('2025-02-01', 820.75),
    ('2025-03-01', 390.50),    ('2025-04-01', 675.25),
    ('2025-05-01', 240.00),    ('2025-06-01', 950.00),
    ('2025-07-01', 485.50),    ('2025-08-01', 730.25),
    ('2025-09-01', 315.00),    ('2025-10-01', 880.75),
    ('2025-11-01', 520.00),    ('2025-12-01', 640.50);
```

### Adjustments Table and one row of data

```SQL
CREATE TABLE Adjustments (
    AdjDate DATE NOT NULL,
    AdjAmount DECIMAL(18, 2) NOT NULL
);
INSERT INTO Adjustments (AdjDate, AdjAmount)
VALUES ('2025-01-01', 100.00)
```

## Semantic Model and Report

Load all three tables into a semantic model and then add relationships and measures. Here is the TMDL

```
createOrReplace

	relationship 1740eeef-4f34-ce80-6411-ee1886e0e726
		fromColumn: Sales.SalesDate
		toColumn: Calendar.Date

	relationship c5da5627-80aa-7642-cd5a-f7340917e603
		fromColumn: Adjustments.AdjDate
		toColumn: Calendar.Date

```

```
createOrReplace

	ref table Adjustments

		measure 'Total Adj' = SUM(Adjustments[AdjAmount])
			formatString: "£"#,0.00;-"£"#,0.00;"£"#,0.00
			lineageTag: 2c0ad192-e385-48c7-b26b-87610aa537b5

			changedProperty = Name

			changedProperty = FormatString

			annotation PBI_FormatHint = {"currencyCulture":"en-GB"}

	ref table Sales

		measure 'Adj Sales' = [Total Sales] + [Total Adj]
			formatString: "£"#,0.00;-"£"#,0.00;"£"#,0.00
			lineageTag: a3f38c8a-a09b-4ae5-b375-3b52d54bc924

			changedProperty = Name

			changedProperty = FormatString

			annotation PBI_FormatHint = {"currencyCulture":"en-GB"}

		measure 'Total Sales' = SUM(Sales[SalesAmount])
			formatString: "£"#,0.00;-"£"#,0.00;"£"#,0.00
			lineageTag: d76c4333-ef6d-499f-9164-c799477246a9

			changedProperty = Name

			changedProperty = FormatString

			annotation PBI_FormatHint = {"currencyCulture":"en-GB"}
```

Also don't forget to sort the Month Name by Month Number


