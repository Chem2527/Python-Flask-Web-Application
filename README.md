# Python-Flask-Web-Application

## Connect to EC2 (For both Ec2's(where we have hosted DB,Application)) using below steps


    1.  Locate the .pem in your local pc and navigate to that folder and open bash 
    
    2.  chmod 400 key1.pem  # (key1.pem is the name of my .pem file)
    
    3.  ssh -i key1.pem ubuntu@ec2-18-222-224-70.us-east-2.compute.amazonaws.com  # (here ec2-18-222-224-70.us-east-2.compute.amazonaws.com  is Public Ipv4  DNS of my Ec2)


## Run below steps in Ec2 where u want to host PostgresDb:

1. sudo apt update -y

2. sudo apt upgrade -y

3. sudo apt install postgresql

4. sudo systemctl start postgresql

5. sudo systemctl status postgresql # (it will show active status)


6.  su - ubuntu # (switch to  ubuntu user - This is mandatory step so wherever your current  path is  just change directory  in upcoming steps u will get to know why)

7.   psql ( It will throw  error psql: error: connection to server on socket "/var/run/postgresql/.s.PGSQL.5432" failed: FATAL:  role "ubuntu" does not exist)

8. sudo -i -u postgres # (Switch to postgres Default user "postgres")

9. psql

10. CREATE ROLE ubuntu WITH LOGIN PASSWORD 'ubuntu';   #(Note create the username with exactly as"ubuntu" as by default only peer authentication is enabled in ec2.)

11. ubuntu@ip-172-31-29-125:/etc/postgresql/16/main$ sudo vi pg_hba.conf # (If someone wants to create a different role name navigate to "/etc/postgresql/<version>/main/" and modify   file pg_hba.conf by looking for  words "all all peer" and modify this to as shown below


"local" is for Unix domain socket connections only
local   all             all                                     md5 )

**md5 changes authentication from local to password based**

 

11. ALTER ROLE ubuntu WITH SUPERUSER; # (Providing root access to ubuntu user- not recommened)


12. \q # (exit postgresql)

13. createdb mydb -O ubuntu

14. exit

15. psql -U ubuntu -d mydb # (For testing purpose we will be establishing a connection to database which is owned by ubuntu user)

16. \l #(it will list )















