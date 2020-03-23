---
title: java使用jdbc连接数据库
tags: java
date: 2018-08-28 16:49:52
---


## java使用jdbc连接mysql的代码
<!-- more -->
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;
import java.sql.ResultSet;


public class testJDBC {
    public static void main(String args[]){
        Connection con;
        String driver="com.mysql.cj.jdbc.Driver";
        String url = "jdbc:mysql://localhost:3306/test?useSSL=false";
        String user = "user";
        String password="password";
        try{
            Class.forName(driver);
            con=DriverManager.getConnection(url,user,password);
            if(!con.isClosed()){
                System.out.println("Connect!");
            }
            Statement statement = con.createStatement();
            String sql = "select * from test";
            ResultSet rs = statement.executeQuery(sql);
            String name=null;
            String sex=null;
            int age=0;
            while (rs.next()){
                name=rs.getString("name");
                sex=rs.getString("sex");
                age=rs.getInt("age");
                System.out.println(name+" "+sex+" "+age);
            }
            rs.close();
            con.close();



        }catch (ClassNotFoundException e){
            System.out.println("Sorry,can`t find the Driver!");
            e.printStackTrace();
        }catch (SQLException e){
            System.out.println("SQLException error");
            e.printStackTrace();
        }catch (Exception e){
            e.printStackTrace();
        }
        finally {
            System.out.println("done");
        }
    }
}
```

在此使用的是`Class.forName(driver)`的办法加载jdbc，当然也可以使用`Driver driver = new com.mysql.jdbc.Driver()`创建driver,不过使用前者可以将数据库解耦出来，因此更好一些，我们可以通过配置文件的形式随时切换数据库。

对于使用`Class.forName`不用加载driver类，是由于JDBC规范中明确要求这个Driver类必须向DriverManager注册自己，因此这一步已经在加载时做到了。