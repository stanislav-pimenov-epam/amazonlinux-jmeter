# amazonlinux-jmeter

## Install jmeter

To install JDK8 and Jmeter 5.1 with plugins on the Amazon Linux instance execute: `sudo install-jmeter.sh`

## Execute Performance Tests from Remote EC2

### Preconditions:
1. Amazon Linux EC2 instance with preinstalled jmeter (like described in the [install-jmeter.sh](https://github.com/stanislav-pimenov-epam/amazonlinux-jmeter/blob/master/install-jmeter.sh))
2. You have SSH key file (.pem) which was used for EC2 instance creation

### Upload test plan

Upload JMX test plan to home directory on EC2 instance

`scp -i [pem_file_path] [jmx_path] ec2-user@<ec2-instance-internal-ip>:~` 

### Execute test plan

##### 1. Connect by SSH

`ssh -i [pem_file_path] ec2-user@<ec2-instance-internal-ip>`

##### 2. Start test execution

2.1 Jmeter command line options

`jmeter -n -t [jmx_path] -l results/results.jtl -e -o results`

* -n flag means “run JMeter in Non-GUI mode”
* -t is used to specify path to jmx file
* -l specifies name of JTL file to log sample results to
* -e generate report dashboard after load test
* -o output directory where to generate the report dashboard after load test. Directory should not exist or be empty

2.2  Example

`nohup jmeter -n -t servicename.jmx -l results/servicename/summary.jtl -e -o results/servicename &`

**NOTE**: do not use `nohup [command] &` if you want to monitor summary in the runtime

##### 3. Download test execution results

3.1 Archive the results to decrease data size to transfer:

`zip -r servicename.zip results/servicename`

3.2 Download to you local machine:

`scp -i [pem_file_path] -r ec2-user@<ec2-instance-internal-ip>:~/servicename.zip [path_to_save]`

## Jmeter reporting

If you need additional charts and reports except those which are provided with the help of jmeter commandline options
then add output into csv file like shown on the screenshot for `Response time` report. 

![](img/jmeter-reporting.png)

## Useful links

In order to make a RPS based testing use [https://www.blazemeter.com/blog/using-jmeters-throughput-shaping-timer-plugin](https://www.blazemeter.com/blog/using-jmeters-throughput-shaping-timer-plugin)