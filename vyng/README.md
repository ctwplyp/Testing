Testing of API Website: http://api-dev.vyng.me/

Task:
As the backend team lead, your CTO has some concerns about the scalability of our infrastructure.  He has asked you to recommend and prototype a load testing solution.  Consider that we are a startup and cost is a concern.  

The Swagger spec for the API to load test is here: http://api-dev.vyng.me/api-docs/
This is the actual dev server API for Vyng.  We have a Postman runner that tests the functionality of the API, but we are not sure how well it will scale under load.  

Submit your recommendation for a load testing solution (3rd party services are acceptable) and an estimate of the costs to run the load tests.  Bonus points for actually setting up a prototype test of a few of the endpoints and reporting results.

Solution:
For this project, I looked at three 3rd party free solutions and one paid solution.  Two other solutions of creating your own could be used if needed.  JMeter came up the best as being free easy to setup, lots of documentation and able to do all API testing needs.  If JMeter didn't fit all the needs it was very easy to upgrade to Blazemeter using the JMeter setups.  See below for details.

Free load testing using 3rd party applications.
1. JMeter, https://jmeter.apache.org/, see JMeter Testing below for more info.
2. ab http://httpd.apache.org/docs/2.2/programs/ab.html, 
3. Tsung,http://tsung.erlang-projects.org/1/01/about/, 
Paid online solution
Blazemeter

Recommend JMeter because easily setup for threads, timing, outputs, header info, https, stepping your requests and variables.  You can also connect it to a network of other computers to use there resources for testing as well.
ab is more of a manual setup but is quick to setup but doesn't have the functionality that Jmeter has.  Most people consider it not a full loading balancing solution but could be combined with catch scripting for more functionality.  
Tsung(http://tsung.erlang-projects.org/,)
Allows you to test HTTP and can send queries to XMPP (Jabber), PostgreSQL, MySQL, LDAP, AMQP, and MQTT
but hasn't been used as much as JMeter and has significantly less documentation.
BlazeMeter 
Online solution, https://www.blazemeter.com/
Seems to have everything very easy to setup.  Runs a little slow but might be a good solution after setting up Jmeter if that is not enough for the site.

JMeter Testing
See the jmx file in the JMeter folder for some apis setup.
See the data folder for csv output of runs
To run install jmeter on a mac use these instructions:
brew install jmeter
If you don't have java please install: https://www.java.com/en/download/help/download_options.xml
To run the test plan in the console go to the folder you saved the testplan and run the following:
jmeter -n -t vyngTestPlanJmeter20180422.jmx -l log.jtl
To edit settings in the jmeter gui:
jmeter
Then open the jmx file, follow instruction on the jmeter site to add APIs and change the number of threads, etc.
https://jmeter.apache.org/usermanual/get-started.html

Ab Testing
To install:
apt-get install apache2-utils
To run use the following:
ab -n <num_requests> -c <concurrency> <addr>:<port><path>
e.g.
ab -n 100 -c 10 -H 'accept: application/json' http://api-dev.vyng.me/api/media/channels

Tsung Testing(http://tsung.erlang-projects.org/, Free )
The instructions and tutorials for setting up were not well writtern.  The configuration is in xml and it ended up being not worth the effort figuring it out.  See github for setting and config.
To install:
brew install tsung
To run:
tsung -f vyngTsung.xml start 

Blazemeter
Non-free solution, https://www.blazemeter.com/pricing
Very easy to setup could use config files from other Jmeter runs.
See results of test
https://a.blazemeter.com/app/?public-token=qXXPpo6FzD0M8X01Q4hn8Happ2qqz7gCAElq606Gq8SP91o5T8#/accounts/219951/workspaces/213876/projects/274858/masters/17540878/summary

Other solutions not thirdy party that could be setup in probably less then a month with just developer time:
First solution would be to setup a Go(could use this for reference, https://github.com/cmpxchg16/gobench) or Java application that sent multiple thread requests to the server for each of the API end points.  Then create metrics with this data or save to a csv spreadsheet for similar analysis as the above solution.  Again should take less then a month to setup with detailed metrics.  The nice part about this is setting memory and number of threads. 

 Batch scripts that sends curl request and records the time of each request to a csv file. Either another batch script or other code would parse the csv file and return metrics.  Instead of that we could open this csv in excel and use pivot tables, functions, macros or straight vba to calculate metrics.  The advantage of this is if the request data needs to be cleaned and you wanted to combine or compare it with other timing data from the load balancer, mongo query times, or log times.  This solution takes developer time but should not cost anything on the  
