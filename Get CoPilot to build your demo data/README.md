# Get CoPilot to build your demo data

## Description

How can we get co-pilot to script the building of the data for our demos from start to end

## Calendar

| Event | Date | Presentation |
| --- | --- | --- |
| Devon and Cornwall Power BI User Group | 3 March 2026 |  [Slides](<2026-03-03 Copilot build my demo data.pdf>) |

## Demos

### Create Repo Prompt

When creating a repo you get a prompt on what the repo is for. Entering in the prompt we then click OK and let it get on with it.

```
I am a Power Query trainer. I have a 2 hour session to teach transformations. 
This repository is to hold the data files that the delegates can download to do the exercises.
I want csv files for customer and products and a folder of monthly csv files for 2025 for orders.

The products are to be herbs and spices and the customers are restaurants and caterers. Make the data realistic.

I need the data to include data that needs the common transformations like split columns,
numbers formatted as text, removing time from a data and others. Please provide details of the
expected transformations to help me prepare for the session
```

### Visual Studio Code Prompt

Rather than using GitHub lets just work in Visual Studio code. Use the panel on the right. There will be alterations to be made via extra prompts.

```
Starting a project for running a training course. I need demo data that needs transforming into a POwer BI report.
I need csv files for customers, products, calendar and orders. The orders should be in seperate files that need combining.
I need basic examples of transformations included and notes to help me run the training course
```

### Write SQL Statements to build a Fabric Warehouse

Ask copilot to create each script, how do we correct what it does? How do we set standards for future scripts.
