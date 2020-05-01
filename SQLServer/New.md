```java
config.properties

db.host=localhost
db.port=1433
db.name=test
db.user=sa
db.password=12345678
```

```java
package config;

import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.util.Properties;

/**
 *
 * @author AnhDT
 */
public class ConnectURL {

    private static String HOSTNAME;
    private static String PORT;
    private static String DBNAME;
    private static String USERNAME;
    private static String PASSWORD;

    static {
        try (InputStream input = new FileInputStream("config.properties")) {

            Properties properties = new Properties();
            properties.load(input);
            
            // get the property value
            HOSTNAME = properties.getProperty("db.host");
            PORT = properties.getProperty("db.port");
            DBNAME = properties.getProperty("db.name");
            USERNAME = properties.getProperty("db.user");
            PASSWORD = properties.getProperty("db.password");
        } catch (IOException ex) {
            ex.printStackTrace();
        }
    }

    /**
     * Get connection string to MSSQL Server
     *
     * @return String
     */
    public static String getConnectionString() {

        // return the connection string.
        return "jdbc:sqlserver://" + HOSTNAME + ":" + PORT + ";"
                + "databaseName=" + DBNAME + ";"
                + "user=" + USERNAME + ";"
                + "password=" + PASSWORD;
    }
}
```

```java
package main;

import config.ConnectURL;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

/**
 *
 * @author AnhDT
 */
public class Main {
    
    public static void main(String[] args) {
        
        String sql = "SELECT * FROM account";
        
        try (Connection con = DriverManager.getConnection(ConnectURL.getConnectionString());
                PreparedStatement ps = con.prepareStatement(sql);) {
            ResultSet rs = ps.executeQuery();
            
            // Iterate through the data in the result set and display it.
            while (rs.next()) {
                System.out.format("%9s %9s\n", rs.getString("username"), rs.getString("password"));
            }
            
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```
