AppScan Dynamic Analyzer
===================
AppScan Dynamic Analyzer identifies security issues in web applications and helps keep them secure. It provides a downloadable report which contains the detected vulnerabilities.

In this tutorial, you will learn how to use AppScan Dynamic Analyzer to scan web applications for vulnerabilities. To further realize the importance of this service, you will first try out the vulnerabilities in the web application.

----------
##### **Download the Web Application** #####
Download a copy of the web application (AppScan.war) that you will deploy in your Bluemix account.

 1. Create a directory `appscan` in the root directory.
 2. Download [AppScan.war](https://github.com/ongj/appscan/raw/master/build/libs/AppScan.war) and save it in `appscan` directory.

----------

##### **Deploy the Web Application in Bluemix using the `cf` tool** #####
 1. Open a terminal window and go to `appscan` directory.

 2. Login to your Bluemix account using the `cf` tool. You will be asked for your credentials, enter your e-mail address and password of your Bluemix account.
> `cf login -a https://api.ng.bluemix.net -s dev`

 3. Upload the web application to your Bluemix account.
 > `cf push appscan-<your_name> -m 256M -p AppScan.war`

	**Example:**
> `cf push appscan-ong -m 256M -p AppScan.war`

 4. After uploading, the application you pushed will be available under the `Applications` section in the Dashboard.

----------

##### **Add and Bind Services to Web Application** #####

This web application uses 2 services, PostgreSQL and AppScan Dynamic Analyzer. 

 1. In the Applications section, click the widget of your application `appscan-<your name>`.
 2. Click `ADD A SERVICE OR API`. This will redirect you to the `CATALOG` page.
> **PostgreSQL**
> 
> 1. Scroll to the bottom of the `CATALOG` page, and click `Bluemix Labs Catalog`.
> 
> 2.  Look for `postgresql` service and click it.
>
> 3.  Change the service name to any name you want and click `CREATE`.
> 
> **AppScan Dynamic Analyzer**
> 
> 1. In the `CATALOG` page, look for `AppScan Dynamic Analyzer` service and click it.
>
> 2. Change the service name to any name you want and click `CREATE`.
> 
> ##### **NOTE:** #####
> After you add a service to the application, you will be asked if you want to restage the application. You can cancel the first notification and restage the application after you have added the 2 services mentioned.

 3. Restage the web application.

----------
##### **Understanding the Communication between the Web Application and the Services** #####

As mentioned earlier, the web application uses 2 services, PostgreSQL and AppScan Dynamic Analyzer.

For the PostgreSQL service, the web application uses this service to save the accounts in a database. Initially, when the web application is first launched, the database is programmatically created and 2 rows or accounts are automatically inserted to be used for this tutorial.

If you extract the contents of the `AppScan.war` file, inside the `WEB-INF/classes/Servlet` directory, you will see the `PostgreSQLClient.java`. 

The `PostgreSQLClient.java` contains the initialization process, which includes the `getConnection()` method and the `createTable()` method. The `getConnection` method simply enables the web application to use the credentials to gain connection to the database. The `createTable` method programmatically creates the `Account` table, and inserts the 2 accounts. The `Account` table consists of 4 columns, `username`, `password`, `firstname`, and `lastname`. 

    public void createTable() throws Exception {
        String sql = "CREATE TABLE IF NOT EXISTS Account "
                + "(username varchar(45) NOT NULL PRIMARY KEY, "
                + "password varchar(45) NOT NULL, "
                + "firstname varchar(45), "
                + "lastname varchar(45))"
                + ";";
        String sql2 = "INSERT INTO Account "
                + "(username, password, firstname, lastname) "
                + "VALUES ('admin', 'password', 'Jarrette', 'Ong'), "
                + "('user1', 'password', 'Lance', 'Del Valle');";
        Connection connection = null;
        PreparedStatement statement = null;

        try {
            connection = getConnection();
            statement = connection.prepareStatement(sql);
            statement.executeUpdate();
            statement = connection.prepareStatement(sql2);
            statement.executeUpdate();
        } catch (Exception e){
            System.out.println("Initialization already completed.");
        } finally {
            if (statement != null) {
                statement.close();
            }

            if (connection != null) {
                connection.close();
            }
        }
    }

For the AppScan Dynamic Analyzer, it does not necessarily have a direct communication with the web application. The web application is bound with AppScan Dynamic Analyzer so that the service will be able to scan the web application for vulnerabilities. Later in this tutorial, you will see how this service scans for vulnerabilities and produces a downloadable report.

----------
##### **Launch the Web Application** #####

 1. Go to the `DASHBOARD` of your Bluemix account. On the `Applications`

