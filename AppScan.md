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
 2. Login to your Bluemix account using the `cf` tool.
> `cf login -a https://api.ng.bluemix.net -s dev`

 3. Upload the web application to your Bluemix account.
> `cf push appscan-<your_name> -m 256M -p AppScan.war`


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
##### **Communication between the Web Application and the Services** #####

As mentioned earlier, the web application uses 2 services, PostgreSQL and AppScan Dynamic Analyzer.

For the PostgreSQL service, the web application uses this service to save the accounts in a database. Initially, when the web application is first launched, the database is programmatically created and 2 rows or accounts are automatically inserted to be used for this tutorial.

The PostgreSQLClient.java has a method called createTable() which programmatically creates the `Account` table, and inserts 2 accounts.
