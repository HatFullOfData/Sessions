# Demo Script

## Desktops

* Presentation - PowerPoint
* Demo - Browser in workspace with lakehouse, 3 models and Daryl Agent
* Power BI - Power BI with the Daryl Model open

## Have ready to type

Put these on stream deck buttons or in your clipboard history or other typing tools.

```
For April 2026
- Total Revenue
- Trading days
- Average daily sales
```

## Alfie Demo

::: info Instructions
1. Open Lakehouse
1. Explain data tables
1. Create agent
1. Connect to the Lakehouse
1. Select all tables
1. Ask Question
:::

## Bella Demo

### Short version

::: info Instructions
1. Open Bella semantic model
1. Show measure and formatting
1. Create agent
1. Connect to semantic model
1. Select all tables
1. Ask question
:::

### Longer Version

::: info Instructions
1. Open Lakehouse
1. Create Semantic model with all tables
1. Add relationships
1. Add measure and format result
1. Create agent
1. Connect to semantic model
1. Select all tables
1. Ask question
:::

## Casper Demo

::: info Instructions
1. Open Casper semantic model - exactly the same as Bella
1. Add description to IsOpen column
1. Create agent
1. Connect to semantic model
1. Select all tables
1. Ask question
:::

```
Indicates whether the business is open on this date. Values are Yes or No
```

## Daryl Demo

::: info Instructions
1. Show Daryl Model open in Power BI desktop
1. Show the descriptions on fields
1. Show TMDL script to assist adding descriptions
1. Show Daryl agent and show agent instructions
:::

## Extras

### Improve Alfie Easily

The Alfie agent based off the Lakehouse could be improved by adding in Example Questions to define some terms

### Total Sales
```SQL
SELECT 
    SUM( SalesAmount ) 
FROM fact_sales
```

### Trading Days
```SQL
SELECT 
    COUNT(*) 
FROM dim_date 
WHERE IsOpen = 'Yes'
```

