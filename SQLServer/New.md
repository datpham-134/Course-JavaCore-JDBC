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
