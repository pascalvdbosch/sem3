# ORM

Een **Object Relational Mapper** \(ORM\) maakt de data in een relationele database toegankelijk in een object-georienteerde programmeertaal op een manier die zo goed mogelijk aansluit bij object-orientatie. 

{% tabs %}
{% tab title="Java" %}
```java
import java.sql.*;

public class ZonderORM {
   public static void main(String[] args) {
      String URL = "jdbc:mysql://localhost/STUDENTEN";
      try {
         Connection c = DriverManager.getConnection(URL, "root", "wachtwoord");
         Statement stmt = conn.createStatement();
         String QUERY = "SELECT id, first, last, age FROM Employees";
         ResultSet rs = stmt.executeQuery(QUERY);
         while (rs.next()) {
            System.out.print("ID: " + rs.getInt("id"));
            System.out.print(", Age: " + rs.getInt("age"));
            System.out.print(", First: " + rs.getString("first"));
            System.out.println(", Last: " + rs.getString("last"));
         }
      } catch (SQLException e) {
         e.printStackTrace();
      } 
   }
}
```
{% endtab %}

{% tab title="C\#" %}
```
string queryString = "SELECT tPatCulIntPatIDPk, tPatSFirstname, tPatSName, tPatDBirthday  FROM  [dbo].[TPatientRaw] WHERE tPatSName = @tPatSName";
string connectionString = "Server=.\PDATA_SQLEXPRESS;Database=;User Id=sa;Password=2BeChanged!;";

using (SqlConnection connection = new SqlConnection(connectionString))
{
    SqlCommand command = new SqlCommand(queryString, connection);
    command.Parameters.AddWithValue("@tPatSName", "Your-Parm-Value");
    connection.Open();
    SqlDataReader reader = command.ExecuteReader();
    try
    {
        while (reader.Read())
        {
            Console.WriteLine(String.Format("{0}, {1}",
            reader["tPatCulIntPatIDPk"], reader["tPatSFirstname"]));// etc
        }
    }
    finally
    {
        // Always call Close when done reading.
        reader.Close();
    }
}
```
{% endtab %}
{% endtabs %}

[https://blog.logrocket.com/why-you-should-avoid-orms-with-examples-in-node-js-e0baab73fa5/](https://blog.logrocket.com/why-you-should-avoid-orms-with-examples-in-node-js-e0baab73fa5/)

