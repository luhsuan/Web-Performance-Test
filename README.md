# Website Testing Notes (Stress and Security)

##  HTTP Method: What's the Difference Between GET and POST in Forms?

### GET vs POST Method

If HTTP were the mechanism for sending mail in our physical world, the format of the envelope would be like HTTP. We can call the contents outside the envelope the http-header, and the letter inside the envelope the message-body. HTTP Method is the set of rules you tell the mail carrier.

Suppose GET means you don't put a letter inside the envelope; it's like sending a postcard. You can write the information you want to send on the outside of the envelope (http-header), write as much as you want, and it's cheaper. On the other hand, POST is a way of sending a letter with something inside the envelope (there's content inside the envelope). Not only can you write on the envelope, but you can also put data or files inside the envelope (message-body), and it's more expensive.

When using GET, you directly add the data you want to send using a Query String (a key/value encoding method) to the address (URL) you want to send it to, and then hand it over to the mail carrier. When using POST, you write the sending address (URL) on the envelope, and additionally, you write the data you want to send on another piece of paper, place it inside the envelope, and hand it over to the mail carrier.

## Stress Testing
>Stress testing involves continually applying pressure to a system until it experiences unacceptable delays or failures, revealing the maximum pressure the target system can withstand. Stress testing requires setting up various scenarios to observe the system's performance under different stress levels and workloads. Stress testing includes:
>
#### Testing Items:
* Strength, Traffic: 
Design different traffic tests or operations based on the nature of the target service.
Example: E-commerce platforms need to test high traffic to ensure smooth operation during promotional events.
* Actions:
Include user operations such as login, page browsing, and form submissions, making the testing plan more realistic.
* Data Exchange: 
For services that require data exchange with the server, test data transmission accuracy and response speed according to the defined data format.
* Diversity: 
The testing plan should include various combinations to thoroughly test server status and stability under different scenarios.

#### Test Environment Setup
Use Apache JMeter and run it in an environment with JAVA JDK 8 or higher.

#### Testing Software Tutorial
Refer to the operation tutorial and configuration parameters on the Jmeter website:
https://ithelp.ithome.com.tw/articles/10203900?sc=rss.qu

#### Report Analysis
* Explanation of Terms on the Aggregate Report:
    * #Samples: Total number of requests sent during this test. For example, if you simulate 10 users, each repeating 10 times, this will show as 100.
    * Average： The average response time for each request.
    * Median： Median response time, which is the response time for 50% of users.
    * 90%、95%、99% Line：Response times for the 90th, 95th, and 99th percentiles of users.
    * Min： The minimum response time.
    * Max： The maximum response time.
    * Error%：The percentage of requests with errors during the test.
    * Throughput： By default, it represents the number of requests completed per second.
    * KB/Sec： The amount of data received from the server per second.

## White Box Testing

>White-box testing, also known as transparent-box testing, glass-box testing, or structural testing, is one of the primary methods of software testing. It is also known as structural testing, logic-driven testing, or code-based testing. White-box testing tests the internal structure or operation of the application, rather than its functionality (as in black-box testing).

### Testing Items
* Code Smell (Maintainability domain)可維護性
* Bug (Reliability domain)可靠性
* Vulnerability (Security domain)安全性
* Security Hotspot (Security domain)安全性
### Software Execution Tutorial (Windows)
#### SonarQube
1. According to the official hardware and software requirements:https://docs.sonarqube.org/latest/requirements/requirements/
2. Download and install JDK 11 : https://www.oracle.com/java/technologies/javase-jdk11-downloads.html
3. Download SonarQube: https://www.sonarqube.org/downloads/
4. After extraction, run these three files as an administrator.
![](https://i.imgur.com/OviTAtj.png)
5. Add the JDK path to the wrapper.conf file, then run StartSonar.bat again. Instructions:https://community.sonarsource.com/t/community-version-startsonar-bat-fails-with-sonarqube-requires-java-11-to-run/12204/7
6. Login at http://localhost:9000/ using the default admin credentials (Account: admin, Password: admin).
   
#### SonarScanner
7. Download SonarScanner for analyzing source code: https://docs.sonarqube.org/latest/analysis/scan/sonarscanner/
8. Following the instructions above, create a sonar-project.properties file in the root directory for analyzing code. Define the projectName as the project name and sources as the directory where the source files are located.
9. In the location where you create a new project in SonarQube, copy the script containing the token. Then, open the Windows PowerShell as an administrator in the root directory for analyzing code. Paste and execute the copied script. 
 ![](https://i.imgur.com/5MaZazX.png)
10. In SonarQube, create a new project, generate a token, and add SonarScanner's bin directory to the %PATH% environment variable.
11. After execution, return to the SonarQube main page to view the analysis results.

Official Documentation: https://docs.sonarqube.org/latest/setup/install-server/

Reference tutorial : https://www.itread01.com/content/1542253277.html 
https://www.itread01.com/content/1563847384.html
https://blog.poychang.net/sonarqube-csharp/
https://dotblogs.com.tw/supershowwei/2016/10/30/164450
https://myweb.ntut.edu.tw/~jykuo/train/2017Sonarqube.pdf

### Software Execution Tutorial (Linux)
#### SonarQube
1. Install jdk 11 Linux Debian Package https://www.oracle.com/java/technologies/javase-jdk11-downloads.html
2. After extraction, open conf/wrapper.conf and add the JDK path to the system by including wrapper.java.command=/usr/lib/jvm/jdk-11.0.8/bin/java.
3. Start the service by executing the following commands in the terminal:
```
cd /home/pt/Downloads/sonarqube-8.4.2.36762/bin/linux-x86-64./sonar.sh start
```
4. Login http://localhost:9000/
#### SonarScanner
1. Open the terminal in the root directory where you want to analyze the data.
2. Modify the profile file by entering sudo gedit /etc/profile in the terminal.
3. Add the following lines at the bottom of the opened file and save:
```
export SONAR_SCANNER_HOME=/home/pt/Downloads/sonar-scanner-4.4.0.2170-linux
export PATH=$ {SONAR_SCANNER_HOME}/bin:${PATH}
```
4. To apply the changes, enter the source /etc/profile in the terminal.
5. Then, enter sonar-scanner -h. If you see the content as shown below, it means the setup was successful:
```
usage: sonar-scanner [options]

Options:
 -D,--define <arg>     Define property
 -h,--help             Display help information
 -v,--version          Display version information
 -X,--debug            Produce execution debug output
```
6. Paste the script generated by SonarQube for your project in the root directory for analyzing code. After running the script, you can view the analysis results at http://localhost:9000/.
   
Reference: https://www.itread01.com/content/1550144356.html

## Black Box Testing
>Black-box testing, also known as third-party security testing, primarily involves penetration testing. It simulates external intrusion when only the system's domain and URL are known. The project is based on the 2017 top 10 penetration behaviors published by the international organization OWASP and the content solicitation for government agency penetration testing projects.

Reference : 
https://secbuzzer.co/post/116
https://secbuzzer.co/post/115


