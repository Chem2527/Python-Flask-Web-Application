![image](https://github.com/user-attachments/assets/374f3151-0c7d-4caa-a487-49b6cd73ddb6)![image](https://github.com/user-attachments/assets/374f3151-0c7d-4caa-a487-49b6cd73ddb6)![image](https://github.com/user-attachments/assets/374f3151-0c7d-4caa-a487-49b6cd73ddb6)# Python-Flask-Web-Application

## Connect to EC2 (For both Ec2's(where we have hosted DB,Application)) using below steps

### Note: After creating ec2's open inbound ports( 5000(hosting application on port 5000),(5432 default port for postgresql as application ec2 wants to connect with db ec2 we need to open this port) 
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

10. CREATE ROLE ubuntu WITH LOGIN PASSWORD 'ubuntu';   #(Note: create the username with exactly as"ubuntu" as by default only peer authentication is enabled in ec2.)

11. ubuntu@ip-172-31-29-125:/etc/postgresql/16/main$ sudo vi pg_hba.conf # (If someone wants to create a different role name navigate to "/etc/postgresql/<version>/main/" and modify   file pg_hba.conf by looking for  words "all all peer" and modify this to as shown below


"local" is for Unix domain socket connections only
local   all             all                                     md5 )

**md5 changes authentication from local to password based**

 ### Note: Im using password based authentication so creating role with name "sai" and im going to create database "mydb1"

12. ALTER ROLE sai WITH SUPERUSER; # (Providing root access to ubuntu user- not recommened)


13. \q # (exit postgresql)

14. createdb mydb1 -O sai

15. exit

16. psql -U sai -d mydb1 # (For testing purpose we will be establishing a connection to database which is owned by ubuntu user)

17. \l #(it will list all the databases )

18. Navigate to ubuntu@postgres:/etc/postgresql/16/main and modify   **postgresql.conf** and look for **#listen_addresses = 'localhost'** and replace it with **listen_addresses = '*'**

19.Navigate to ubuntu@postgres:/etc/postgresql/16/main and modify  modify **pg_hba.conf**  and at bottom add "host    mydb1 # (replace with db u created earlier)   # sai(owner of db)    3.129.8.10/32 #(Ipv4 range)    md5 # (enabling pswd authentication)"   --->  overall # (host    mydb1    sai    3.129.8.10/32    md5
)


20. sudo systemctl restart postgresql

21. sudo service postgresql restart










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

14. Incase if port is already listening use below command:

15. sudo lsof -i :5000
COMMAND  PID   USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
python3 6852 ubuntu    4u  IPv4  34193      0t0  TCP localhost:5000 (LISTEN)
python3 6853 ubuntu    4u  IPv4  34193      0t0  TCP localhost:5000 (LISTEN)
python3 6853 ubuntu    5u  IPv4  34193      0t0  TCP localhost:5000 (LISTEN)

16. sudo kill 6852

17. need to add below content to app.py
if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000, debug=True)  # Listen on all network interfaces

Finally we are good to run the application form from browser through p.ip:5000 and once we filled and submitted the form it will show below images


 ![image](https://github.com/user-attachments/assets/e9f68124-2a6f-4d67-aa74-99c45665c213)
![image](https://github.com/user-attachments/assets/e9f68124-2a6f-4d67-aa74-99c45665c213)







   



 











