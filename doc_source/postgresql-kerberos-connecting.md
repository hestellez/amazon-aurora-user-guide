# Connecting to PostgreSQL with Kerberos Authentication<a name="postgresql-kerberos-connecting"></a>

You can connect to PostgreSQL with Kerberos authentication with the pgAdmin interface or with a command line interface such as psql\. For more information about connecting, see   [Connecting to an Amazon Aurora PostgreSQL DB Cluster](Aurora.Connecting.md#Aurora.Connecting.AuroraPostgreSQL)\. 

## pgAdmin<a name="collapsible-section-pgAdmin"></a>

To use pgAdmin to connect to PostgreSQL with Kerberos authentication, take the following steps:

1. Launch the pgAdmin application on your client computer\.

1. On the **Dashboard** tab, choose **Add New Server**\.

1. In the **Create \- Server** dialog box, type a name on the **General** tab to identify the server in pgAdmin\.

1. On the **Connection** tab, type the following information from your Aurora PostgreSQL database:
   + For **Host**, type the endpoint\. For example, use the format *PostgreSQL\-endpoint*\.*AWS\-Region*\.rds\.amazonaws\.com\.
   + For **Port**, type the assigned port\.
   + For **Maintenance database**, type the name of the initial database to which the client will connect\.
   + For **Username**, type the user name that you entered for Kerberos authentication in [Step 6: Create Kerberos Authentication PostgreSQL Logins](postgresql-kerberos-setting-up.md#postgresql-kerberos-setting-up.create-logins)\. 

1. Choose **Save**\.

## psql<a name="collapsible-section-psql"></a>

To use psql to connect to PostgreSQL with Kerberos authentication, take the following steps:

1. At a command prompt, run the following command\.

   ```
   kinit username                
   ```

   Replace *`username`* with the user name\. At the prompt, enter the password stored in the Microsoft Active Directory for the user\.

1. If the PostgreSQL DB cluster is using a publicly accessible VPC, put a private IP address for your DB cluster endpoint in your `/etc/hosts` file on the EC2 client\. For example, the following commands obtain the private IP address and then put it in the `/etc/hosts` file\.

   ```
   % dig +short PostgreSQL-endpoint.AWS-Region.rds.amazonaws.com  
   ;; Truncated, retrying in TCP mode.
   ec2-34-210-197-118.AWS-Region.compute.amazonaws.com.
   34.210.197.118 
   
   % echo " 34.210.197.118  PostgreSQL-endpoint.AWS-Region.rds.amazonaws.com" >> /etc/hosts
   ```

1. Use the following psql command to log in to a PostgreSQL DB cluster that is integrated with Microsoft Active Directory\.

   ```
   psql -U username@CORP.EXAMPLE.COM -p 5432 -h PostgreSQL-instance-endpoint.AWS-Region.rds.amazonaws.com postgres
   ```