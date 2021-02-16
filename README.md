# AWS-NodeJS-Deployment

## Instance Creation

* Create an EC-2 Instance in AWS using Ubuntu 18.04.
* Add Ports of Type SSH , http and https.
* Also Add Custom Ports of the Backend Localhost (Mine is 8080 and 8081).
* Create a new (.pem) file of your instance.

## Elastic IP Address

* The Purpose of Creating this is that everytime we reboot our Instance we are provided with a New IP address for our Site.
* So we create an Elastic IP and Assign it to our Instance Running.

## Start the Server

* In the path where we've downloaded the (.pem) file , open Terminal or Bash 

```bash
chmod 400 <fileName>.pem
ssh -i "<fileName>.pem" ubuntu@ec<Elastic IP>.us-east-2.compute.amazonaws.com
```

* This should start your server.
