# LInux-Configuration-system
what the project aim to ?
here , we want to deploy project to the linux server machine with all requirements such as securing it and updating the database, here the details to accses the project on the internet:

The IP Address: 34.220.97.82
SSH Port: 2200
URL using DNS: ec2-34-220-97-82.us-west-2.compute.amazonaws.com



We should connect to the server machine using these two commands
to access the file which contains the pair key of the srever machine Downloads here is the directory name ahmed_udacity.pem is the file name

chmod 600 ~/Downloads/ahmed_udacity.pem
 ssh -i ~/Downloads/ahmed_udacity.pem ubuntu@34.220.97.82

#software installed

1 - Apache webserver using Flask framework and bootstrap formatting.

2 - Data is saved to a Postgresql database on the server.

#Configurations made

Root access disabled

Postgresql user called 'catalog' added, with password 'password'

An additional superuser called 'grader' with password 'grader01000' was added to allow Udacity grader to access the server

Password access to the server disabled

Access limited to private rsa key authentication

Enabled firewall to restrict ports to 2200 for ssh, 80 for webservice, 123 for NTP. All others ports have been blocked.

The ssh port is 2200 - a non-default port. The normal port (22) has been disabled.

#Third Party Resources

https://github.com/mulligan121/Udacity-Linux-Configuration

https://github.com/mulligan121/Udacity-Linux-Configuration
