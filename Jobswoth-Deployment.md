1. Create a new database and user in postgresql with full permissions to that database.
2. Install Apache Tomcat
3. Deploy https://github.com/ari/jobsworth/releases/download/v4.0b3/ROOT.war in webapps folder.
4. Add the following to tomcat context.xml

~~~
<?xml version="1.0" encoding="UTF-8"?>
<!-- 
This file is an example. You will need to customise it for your own needs, and rename it
to context.xml. In Apache Tomcat is belongs inside a folder called "config".

You can change the mysql driver to postgresql and you will need to replace all the variables
which look like this ${VARIABLE} either at runtime using a script, or by editing this file
directly.
-->

<Context swallowOutput="true">

  <Resource name="jdbc/jobsworth"
            removeAbandoned="true"
            auth="Container" type="javax.sql.DataSource"
            maxActive="100" maxIdle="30" maxWait="10000"
            username="jobsworth" password="jobsworth"
            driverClassName="org.postgresql.Driver" batchSize="-1"
            url="jdbc:postgresql://localhost:5432/jobsworth"
            validationQuery="select 1"/>

  <!-- The base domain of the running application. The actual URL you use to access the system is the name of the tenant followed by this. So you might have 'company1.acme.com' and 'company2.acme.com' -->
  <Parameter name="config.domain" value="arrivuapps.org" override="false"/>

  <!-- Jobsworth can be branded as something else -->
  <Parameter name="config.productName" value="Projects" override="false"/>

  <!-- SSL is good. But you may want to turn it off while you are first setting this up -->
  <Parameter name="config.ssl" value="true" override="false"/>

   <!-- A path to a folder on disk where jobsworth will store task attachments -->
  <Parameter name="config.storeroot" value="/var/jobsworth/assets" override="false"/>

  <!-- What domain will mail from jobsworth come from? -->
  <Parameter name="config.email_domain" value="arrivuapps.org" override="false"/>
<!-- Some emails like password resets will come from this user name. So in this example, it will be jobsworth@acme.com -->
  <Parameter name="config.email_replyto" value="pm@arrivusystems.com" override="false"/>

  <!-- The from user name. This is usually the same as the reply-to username -->
  <Parameter name="config.email_from" value="projects" override="false"/>

  <!-- Something to put at the start of every subject on emails from jobsworth -->
  <Parameter name="config.email_prefix" value="'[Arrivu projects]'" override="false"/>

  <!-- Jobsworth SMTP settings -->
  <Parameter name="config.smtp_host" value="smtp.sendgrid.net" override="false"/>
  <Parameter name="config.smtp_port" value="587" override="false"/>
  <Parameter name="config.smtp_domain" value="arrivuapps.org" override="false"/>
  <Parameter name="config.smtp_authentication" value="login" override="false"/>
  <Parameter name="config.smtp_user_name" value="jigsawacademy" override="false"/>
  <Parameter name="config.smtp_password" value="admin123$" override="false"/>

  <!-- This value is used for integration with your incoming mail server. Change it! -->
  <Parameter name="config.secret" value="oifhaeflnafioeafhiofajhieiwqtoqfoqwhfgwwhQLQBNI" override="false"/>

  <!-- Jobsworth email notification of errors -->
  <Parameter name="config.error_email_prefix" value="'[jobsworth error]'" override="false"/>
  <Parameter name="config.error_sender_address" value="alert@arrivusystems.com" override="false"/>
  <Parameter name="config.error_exception_recipients" value="['alert@arrivusystems.com','samuel@arrivusystems.com', 'babin@arrivusystems.com']" override="false"/>

</Context>
~~~
