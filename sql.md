ADMIN_LOGIN="ServerAdmin"
RESOURCE_GROUP=learn-fbb727b3-d0c6-4246-be97-f7b113cd6a12
SERVERNAME=FitnessSQLServer-$RANDOM
LOCATION=westus2
PASSWORD=intelat123*


### BCP Import

```
bcp "$DATABASE_NAME.dbo.courses" format nul -c -f courses.fmt -t, -S "$DATABASE_SERVER.database.windows.net" -U $AZURE_USER -P $AZURE_PASSWORD
```

### BCP format file

```
courses.fmt

14.0
2
1       SQLCHAR             0       12      ","    1     CourseID                                     ""
2       SQLCHAR             0       50      "\n"   2     CourseName                                   SQL_Latin1_General_CP1_CI_AS
```

### System.Data.SqlClient library

#### Connect 

```
using System.Data.SqlClient;

...

string connectionString = "Server=tcp:myserver.database.windows.net,...";

// Method One
SqlConnection con = new SqlConnection(connectionString);
con.Open();
con.Close();

// Method 2
using (SqlConnection con = new SqlConnection(connectionString))
{
    // Open and Use the connection here
    con.Open();
    ...
}
// Connection is now closed
```

#### Define an SQL command or query
```
SqlCommand deleteOrdersForCustomer = new SqlCommand("DELETE FROM Orders WHERE CustomerID = @custID", con);
deleteOrdersForCustomer.CommandType = CommandType.Text;
string customerID = <prompt the user for a customer to delete>;
deleteOrdersForCustomer.Parameters.Add(new SqlParameter("custID", customerID));
```

#### Run a command
```
int numDeleted = deleteOrdersForCustomer.ExecuteNonQuery();
```

#### Execute a query and fetch data
##### If your SqlCommand contains an SQL SELECT statement, you run it by using the ExecuteReader method. 
##### method returns an SqlDataReader object that you can use to iterate through the results and process each row in turn.
```
SqlDataReader rdr = queryCmd.ExecuteReader();

// Read the data a row at a time
while (rdr.Read())
{
    string firstName = rdr["FirstName"].ToString();
    string lastName = rdr["LastName"].ToString();
    int orderID = Convert.ToInt32(rdr["OrderID"]);

    // Process the data
    ...
}
```

### Handle exceptions and errors
```
...
using (SqlConnection con = new SqlConnection(connectionString))
{
    SqlCommand command = new SqlCommand("DELETE FROM ...", con);
    try
    {
        con.Open();
        command.ExecuteNonQuery();
    }
    catch (SqlException ex)
    {
        for (int i = 0; i < ex.Errors.Count; i++)
        {
            Console.WriteLine($"Index # {i} Error: {ex.Errors[i].ToString()}");
        }
    }
}
```
