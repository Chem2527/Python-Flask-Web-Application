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

16. \l #(it will list all the databases )

17. Navigate to ubuntu@postgres:/etc/postgresql/16/main and modify   **postgresql.conf** and look for **#listen_addresses = 'localhost'** and replace it with **listen_addresses = '*'**

18.Navigate to ubuntu@postgres:/etc/postgresql/16/main and modify  modify **pg_hba.conf**  and at bottom add "host    mydb1 # (replace with db u created earlier)   # sai(owner of db)    3.129.8.10/32 #(Ipv4 range)    md5 # (enabling pswd authentication)"   --->  overall # (host    mydb1    sai    3.129.8.10/32    md5
)


19. sudo systemctl restart postgresql

20. sudo service postgresql restart







## Run below commands in Ec2  where we are hosting  the  Python-Flask-Web-Application:
1. sudo apt update


2.  clone the repo using **git clone https://github.com/Chem2527/Python-Flask-Web-Application.git**  # (ensure git is installed)

3. cd Python-Flask-Web-Application/


4. python app.py(it will throw error as we didnt installed python)

5. sudo apt install python3

6. python3 --version (shows the installed version for me 3.12)


7. sudo add-apt-repository ppa:deadsnakes/ppa

8. sudo apt update

9. sudo apt install python3-pip

10. python3 app.py # (shows error stating ModuleNotFoundError: No module named 'flask')

11. sudo apt-get install python3-flask

12. sudo apt-get install python3-flask-sqlalchemy


13. sudo apt install python3-psycopg2

14. 







 











