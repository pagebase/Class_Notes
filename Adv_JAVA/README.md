> How to change `workspace`?

files--> switch workspace

> How to reset window

windows--> Perspective--> Reset perspective

> How to create project?

File--> new--> other--> java project--> next--> `enter your project name`--> finish--> open perspective.

> How to add packages, inteface and class?

Right click on project--> New--> choose `class` or `packages` or `inteface`--> Enter `class` name--> mark check on `public static void main(String[] args)`--> finish.

> How to configure `OJDBC 14` `JAR` file for oracle database?

Right click on the project--> select `build path`--> click on `confifure build path`--> click on `library` tab--> click on `Add ectrenal JARs`--> choose `OJDBC 14`
	> Directory to find `JAR` file. `C:\oraclexe\app\oracle product\10.2.01\Server\jdbc\lib`

> How to register driver?

```java
import java.sql.*;

public class Grandpa {

	public static void main(String[] args) throws SQLException {
	System.out.println("Register Driver");
DriverManager.registerDriver(new oracle.jdbc.OracleDriver()); // Register Driver
	System.out.println("Register sucessfully");
	Connection con=DriverManager.getConnection("jdbc:oracle:thin:@localhost:1521:xe","system","password"); // Connection established
	System.out.println("Connection successfullly done");	
	
	Statement st=con.createStatement(); // Statement created
	PreparedStatement ps= con.prepareStatement("insert into Student values(?,?)");
	ps.setInt(1, 1);
	ps.setString(2, "Sham");
	ps.executeUpdate();
	
	System.out.println("Table created!");
	
	}
}
java table data insertion
```

---
