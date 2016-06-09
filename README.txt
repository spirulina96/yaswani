Case 1: PRODSUP-001 =>

Customer Query :  
   Hi. I am trying to get my test cases to show up on CircleCI but it is not working. I have a fork of this project in my github account https://github.com/mtedone/podam. I added the project to CircleCI and got a green build but the Test Results tab is empty
      ---
      Harry

Solution :

      Hi Harry, 
      
      Thanks for contacting CircleCi support today. 
      
      As I understand: You added a java project from github ( https://github.com/mtedone/podam) to Circleci and after successful or  green build  the Test Results tab is empty. 
      
      In order to replicate the issue at my end, I did a fork of code built it using CircleCI . I found out that code is missing a circle.yml in its root directory, also it’s a maven build which needs a maven surefire plugin to generate test report
      
      Here is a link : https://circleci.com/docs/test-metadata/#java-junit-results-with-maven-surefire-plugin
      
      Please create a circle.yml file in the root directory of the project with following content and re-run:
      
            test:
              override:
                - mvn clean install
              post:
                - mkdir -p $CIRCLE_TEST_REPORTS/junit/
                - find . -type f -regex ".*/target/failsafe-reports/.*xml" -exec cp {} $CIRCLE_TEST_REPORTS/junit/ \;
      
      Please feel free to revert if issue persist or for any other queries regarding this.
      
      Thanks Again
      Regards
      Yaswani
      
      
=============================
Steps Performed :
=============================
1) Branch Forked
2) Build run 
3) No test results 
4) Missing circle.yml file for mvn (maven build)
5) created circle.yml file with following code 
      test:
       post:
         - mkdir -p $CIRCLE_TEST_REPORTS/junit/
         - find . -type f -regex ".*/target/surefire-reports/.*xml" -exec cp {} $CIRCLE_TEST_REPORTS/junit/ \;

6) Build agian but no result in test 
7) checked surefire plugin in pom.xml and found following "<skip>true</skip>"
            <plugin>
   				<groupId>org.apache.maven.plugins</groupId>
   				<artifactId>maven-surefire-plugin</artifactId>
   				<version>2.11</version>
   				<configuration>
   				<skip>true</skip>
   				</configuration>
   			</plugin>
8) removed <skip>true</skip>
9) build again and got successful this time


Here is my work :
               https://github.com/spirulina96/podam 
               https://circleci.com/gh/spirulina96/podam/8


--------------------------------------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------------------------------------
Case 2: PRODSUP-002 =>

Customer Query :
      Hi. I added my project to CircleCI but I can’t get the build complete. I’m not sure what I’m doing wrong. Can you help? My project is https://github.com/nym2015/commons-csv.
      ---
      Jolene


Solution :

   Hi Jolene,
   
   Thanks for contacting CircleCi support team today.
   
   As I understand that you are facing an issue with the project in CirceCi and is not getting a build complete status.
   
   In order to understand the issue, I need to replicate the same at my end. hence did a fork of code as its publically available.
   
   I did a build and got stuck there for 15-20 mins but build wasn’t successful It was executing slowly with following message on the dashboard
   
            -------------------------------------------------------
             T E S T S
            -------------------------------------------------------
            Running org.apache.commons.csv.CSVFormatTest
            .................................................
   
   Hence I did check the CSVFormat.java file and found out that following file around line 795 has a for loop printing dots for (20*60*60)/ 72000 times and has sleep call as well, this is where the whole process execution seems to be stuck. 
         ***********   
            commons-csv/src/main/java/org/apache/commons/csv/CSVFormat.java  : 795
         ***********
            /**
            * Verifies the consistency of the parameters and throws an IllegalArgumentException if necessary.
            *
            * @throws IllegalArgumentException
            */
            private void validate() throws IllegalArgumentException {
            for (int i=0; i<20*60*60; i++) {
                System.out.print('.');
                try {
                    Thread.currentThread().sleep(1000);
                } catch (InterruptedException e) {
                    break;
                }
            }
   
   I believe the reason for build failure is above code which runs for 72000 times and goes way above from default timeout value of 600s. Built was successful after I removed the above block form the code. 
   
   It is also mentioned here:  https://circleci.com/docs/configuration/
   
   Please revert if you have any other issue reading the project or any suggestion for the above solution.
   
   Reagrds
   Yaswani


=============================
Steps Performed :
=============================

1)Branch Forked 
2)Build runs
3)it hangs
4)check changes errors or output : T E S T S => Running org.apache.commons.csv.CSVFormatTest
5) Checked the src for CSVFormatTest.java
6) Found a for loop run 72000 times and sleep wait(1000) - try to speed up
7) looks like the method is called many times checked the default value of command exectuion for circleCI(600s)
8) Tried to negate the loop and build again 
         private void validate() throws IllegalArgumentException {
                 /*
                 for (int i=0; i<20*60*60; i++) {
                     System.out.print('.');
                     try {
                         Thread.currentThread().sleep(1000);
                     } catch (InterruptedException e) {
                         break;
                     }
                 }
                 */
8) Build successful after removing above code.

Here are my changes and circleci build:

   https://github.com/spirulina96/commons-csv 
   https://circleci.com/gh/spirulina96/commons-csv/5

--------------------------------------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------------------------------------

Case 3: PRODSUP-003 =>

Customer Query :
      Dear customer support,
      
      I need to explain how CircleCI works to my boss. I’ve read the docs but I still don’t get it. Can you please explain to me, in your own words, how CircleCI works? Also, it seems like you don’t support .NET builds. Why is that?
      --
      Adam

Solution :

Hi Adam, 

Thanks for contacting CircleCi Support today. 
My name is Yaswani and am happy to help you today with the product features and query about .Net 
 
CircleCI is the Continuous Integration tools generally used at software development shops across the IT industry, to build packages, integration of test suites and deployment. It’s a complete CI&CD tool which can perform end to end automation of software packages from code commit to deployment to production following the SDLC 

CicrleCi is pretty simple to apply and just work as plug and play with most of the standard projects. In case, if your project need some special setup then there are many possible ways and integrations to suit different type of needs.

One can configure it to watch Git repo and build on latest commits which includes test and on success it will get deployed on to designated server or environment.
 
At the moment CircleCi support 2 mobile platforms, 8 web languages, continuous work is going on to add support for other languages and .Net is one of the them which will be available soon.

I recommend you to visit the following site for its features and know how as
support and documentation is readily available here : https://circleci.com/docs. 

Please feel free to revert for any more queries about the product/ suggestions/ any technical issues.

Regards
Yaswani 




