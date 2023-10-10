# Использование JDBC Postgres

[Документация по использованию JDBC Postgres](https://jdbc.postgresql.org/documentation/)

После [[Подключение JDBC драйвера|подключения JDBC драйвера Postgres]] его можно использовать. пример:

```java
public class Test {

    public static void main(String[] args){
        try {
            String url = "jdbc:postgresql://localhost:5432/test";
            String user = "user";
            String passwd = "password";
            Connection conn = DriverManager.getConnection(url, user, passwd);

            Statement st = conn.createStatement();
            ResultSet rs = st.executeQuery("select * from users");

            while (rs.next()) {
                System.out.print("Column 1 returned ");
                System.out.println(rs.getString(1));
            }

            rs.close();
            st.close();
            conn.close();
        } catch (SQLException ex) {
            System.out.println("Error: " + ex.getMessage());
        }
    }
}
```