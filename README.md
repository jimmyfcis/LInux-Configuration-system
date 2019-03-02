# Linux Server Configuration Project

### what the project aim to ?
> here , we want to deploy project to the linux server machine with all requirements such as securing it and updating the database, here the details to accses the project on the internet: 

* The IP Address: [34.220.97.82](http://34.220.97.82/)
* SSH Port: 2200
* URL using DNS: [ec2-34-220-97-82.us-west-2.compute.amazonaws.com](http://ec2-34-220-97-82.us-west-2.compute.amazonaws.com/)

### We should connect to the server machine using these two commands
to access the file which contains the pair key of the srever machine
`Downloads` here is the directory name
`ahmed_udacity.pem` is the file name
```
chmod 600 ~/Downloads/ahmed_udacity.pem
```
```
 ssh -i ~/Downloads/ahmed_udacity.pem ubuntu@34.220.97.82
```

### How to configure the server ? 

#### 1. we should update all packages
```
sudo apt-get update
```
```
sudo apt-get upgrade
```
```
sudo apt-get dist-upgrade
```

 and we should enable automatic security updates
```
sudo apt-get install unattended-upgrades
```
```
sudo dpkg-reconfigure --priority=low unattended-upgrades
