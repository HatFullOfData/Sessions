# Code from Demos

## Hello World

```python
import datetime
import fabric.functions as fn
import logging

udf = fn.UserDataFunctions()

@udf.function()
def hello_fabric(name: str) -> str:
    logging.info('Python UDF trigger function processed a request.')

    return f"Welcome to Fabric Functions, {name}, at {datetime.datetime.now()}!"
```

## Add Employee

```python
# Select 'Manage connections' and add a connection to a Fabric SQL Database 
# Replace the alias "<alias for sql database>" with your connection alias.
@udf.connection(argName="sqlDB",alias="M365Finance")
@udf.function()
def write_one_to_sql_db(sqlDB: fn.FabricSqlConnection, employeeId: int, employeeName: str, deptId: int) -> str:
    '''
    Description: Insert one employee record into SQL database, creating table if needed.
    
    Args:
        sqlDB (fn.FabricSqlConnection): Fabric SQL database connection.
        employeeId (int): Employee ID (primary key).
        employeeName (str): Employee name.
        deptId (int): Department ID.
    
    Returns:
        str: Confirmation message about table creation and data insertion.
        
    '''

    # Replace with the data you want to insert
    data = (employeeId, employeeName, deptId)

    # Establish a connection to the SQL database
    connection = sqlDB.connect()
    cursor = connection.cursor()
  
    # Create the table if it doesn't exist
    create_table_query = '''
        IF OBJECT_ID(N'dbo.Employee', N'U') IS NULL
        CREATE TABLE dbo.Employee (
            EmpID INT PRIMARY KEY,
            EmpName nvarchar(50),
            DepID INT
            );
    '''
    cursor.execute(create_table_query)
 
    # Insert data into the table
    insert_query = "INSERT INTO Employee (EmpID, EmpName, DepID) VALUES (?, ?, ?);"
    cursor.execute(insert_query, data)

    # Commit the transaction
    connection.commit()

    # Close the connection
    cursor.close()
    connection.close()               
    return "Employee table was created (if necessary) and data was added to this table"
```

## Add Adjustment

```python
@udf.connection(argName="sqlDB",alias="M365Finance")
@udf.function()
def add_adjustment(sqlDB: fn.FabricSqlConnection, date: str, amount: float) -> str:
    try:
        '''
        Description: Insert one adjustment record into SQL database adjustments table.
        
        Args:
            sqlDB (fn. FabricSqlConnection): Fabric SQL database connection.
            date (str): Adjustment date (format: YYYY-MM-DD).
            amount (float): Adjustment amount.
        
        Returns:
            str: Confirmation message about data insertion.
            
        '''

        # Data to insert
        data = (date, amount)

        # Establish a connection to the SQL database
        connection = sqlDB.connect()
        cursor = connection.cursor()
    
        # Insert data into the adjustments table
        insert_query = "INSERT INTO adjustments (AdjDate, AdjAmount) VALUES (?, ?);"
        cursor.execute(insert_query, data)

        # Commit the transaction
        connection. commit()

        # Close the connection
        cursor.close()
        connection.close()               
        return "Adjustment record was successfully added to the adjustments table"
    except Exception as e: 
        return f"Error: {str(e)}"
    ```