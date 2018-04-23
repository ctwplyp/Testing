Testing of API Website: http://api-dev.vyng.me/

Task:
As the backend team lead, your CTO has some concerns about the scalability of our infrastructure.  He has asked you to recommend and prototype a load testing solution.  Consider that we are a startup and cost is a concern.  

The Swagger spec for the API to load test is here: http://api-dev.vyng.me/api-docs/
This is the actual dev server API for Vyng.  We have a Postman runner that tests the functionality of the API, but we are not sure how well it will scale under load.  

Submit your recommendation for a load testing solution (3rd party services are acceptable) and an estimate of the costs to run the load tests.  Bonus points for actually setting up a prototype test of a few of the endpoints and reporting results.

Solution:
Free load testing using 3rd party applications.
1. JMeter, https://jmeter.apache.org/, see JMeter Testing below for more info.
2. ab http://httpd.apache.org/docs/2.2/programs/ab.html, 
3. Tsung,http://tsung.erlang-projects.org/1/01/about/, 
4. Siege

Recommend Jmeter because easily setup for threads, timing, outputs, header info, https, stepping your requests and variables.  You can also connect it to a network of other computers to use there resources for testing as well.
ab is more of a manual setup and doesn't have the GUI mode that JMeter does plus sends all the request in one big go.  
Tsung(http://tsung.erlang-projects.org/,)
Not only allows you to test HTTP but also can send queries to all of below:
XMPP (Jabber)
PostgreSQL
MySQL
LDAP
AMQP
MQTT
but hasn't been used as much as JMeter and has significantly less documentation.


JMeter Testing
See the jmx file in the JMeter folder for some apis setup.
See the data folder for csv output of runs
To run install jmeter on a mac use these instructions:
brew install jmeter
To run the our test plan in the console got to the folder you saved the testplan and run the following:
jmeter -n -t vyngTestPlanJmeter20180422.jmx -l log.jtl
To edit settings in jmeter:
jmeter
Then open the jmx file, follow instruction on the jmeter site to add APIs and change the number of threads.
https://jmeter.apache.org/usermanual/get-started.html

Ab Testing


Tsung Testing(http://tsung.erlang-projects.org/, Free,  )
Not only allows you to test HTTP but also can send queries to all of below:
XMPP (Jabber)
PostgreSQL
MySQL
LDAP
AMQP
MQTT




Other solutions not thirdy party that could be setup:
  software side.  For the vyng api this could be setup in less then a month with detailed metrics.
First solution would be to setup a Go(could use this for reference, https://github.com/cmpxchg16/gobench) or Java application that sent multiple thread requests to the server for each of the API end points.  Then create metrics with this data or save to a csv spreadsheet for similar analysis as the above solution.  Again should take less then a month to setup with detailed metrics.  The nice part about this is setting memory and number of threads. 

 Batch scripts that sends curl request and records the time of each request to a csv file. Either another batch script or other code would parse the csv file and return metrics.  Instread of that we could open this csv in excel and use pivot tables, functions, macros or straight vba to calculate metrics.  The advantage of this is if the request data needed to be cleaned and you wanted to combine it with other timing data from the load balancer, mongo query times, and different code part times.  This solution takes developer time but should not cost anything on the  
