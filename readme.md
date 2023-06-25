# Real Estate Price Prediction - Bangalore
![Project_pic 2](https://github.com/pragti-chauhan/Real-Estate-Price-Prediction/assets/66918663/c55f9696-a430-4a3d-ba80-d7a090af319b)

## About
This is a data science project, where aim is to predict property prices of a place (Bangalore in this case) using features like location, property area, bhk, etc. Prediction is done using a machine learning classifier and project has a web development component based on Python Flask server, and deployed over cloud (Amazon EC2).

## Phases
Below is the step by step process of how to build a Real Estate Price Prediction website :-
- First phase is the model building phase. Some of the steps performed during this phase include- data loading and cleaning, outlier detection and removal, feature engineering, etc. Model is built using sklearn and trained on Linear Regression Classifier using Bangalore Home Prices dataset, taken from kaggle.com.
- Second would be to write a python flask server that uses the saved model to serve http requests.
- Third component is the website built in html, css and javascript that allows user to enter location, square ft area, bedrooms etc and it will call python flask server to retrieve the predicted price.

## Deploying the app to cloud (AWS EC2)

1. Create EC2 instance using Amazon Console, also in security group add a rule to allow HTTP incoming traffic
2. Now connect to your instance using a command like this,
```
ssh -i "C:\Users\Pragti\.ssh\BHP.pem" ec2-13-51-200-148.eu-north-1.compute.amazonaws.com
```
3. nginx setup
   - Install nginx on EC2 instance using these commands,
   ```
   sudo apt-get update
   sudo apt-get install nginx
   ```
   - Above will install nginx as well as run it. Check status of nginx using
   ```
   sudo service nginx status
   ```
   - Here are the commands to start/stop/restart nginx
   ```
   sudo service nginx start
   sudo service nginx stop
   sudo service nginx restart
   ```
 4. Now, copy all your code to EC2 instance. This can be done, either by using git or by copying files using WinSCP. I have used WinSCP. WinSCP can be downloaded from here: https://winscp.net/eng/download.php
 5. Once you connect to EC2 instance from WinSCP, you can now copy all code files into /home/ubuntu/ folder. The full path of your root folder is now: **/home/ubuntu/BangloreHomePrices**
 6.  After copying code on EC2 server, now we can point nginx to load our property website by default. Follow below steps :-
     1. Create this file /etc/nginx/sites-available/bhp.conf. The file content looks like this,
     ```
     server {
 	    listen 80;
             server_name bhp;
             root /home/ubuntu/BangloreHomePrices/client;
             index app.html;
             location /api/ {
                  rewrite ^/api(.*) $1 break;
                  proxy_pass http://127.0.0.1:5000;
             }
     }
     ```
     2. Create symlink for this file in /etc/nginx/sites-enabled by running this command,
     ```
     sudo ln -v -s /etc/nginx/sites-available/bhp.conf
     ```
     3. Remove symlink for default file in /etc/nginx/sites-enabled directory,
     ```
     sudo unlink default
     ```
     4. Restart nginx,
     ```
     sudo service nginx restart
     ```
 7. Now install python packages and start flask server
 ```
 sudo apt-get install python3-pip
 sudo pip3 install -r /home/ubuntu/BangloreHomePrices/server/requirements.txt
 python3 /home/ubuntu/BangloreHomePrices/client/server.py
 ```
 Running last command above will prompt that server is running on port 5000.
 8. Now just load your cloud url in browser (for me it was http://ec2-13-51-200-148.eu-north-1.compute.amazonaws.com/) and this will be fully functional website running in production cloud environment

## Tech Stack

1. Python
2. Numpy and Pandas for data cleaning
3. Matplotlib for data visualization
4. Sklearn for model building
5. Google Colaboratory, Atom and pycharm as IDE
6. Python flask for http server
7. HTML/CSS/Javascript for UI

## Acknowledgements

 - [Awesome Readme Templates](https://awesomeopensource.com/project/elangosundar/awesome-README-templates)
 - [Referrence for the project](https://github.com/codebasics/py/blob/master/DataScience/BangloreHomePrices/readme.md)
 - [Cloud Deployment](https://aws.amazon.com/console/)

## 🔗 Links
[![linkedin](https://img.shields.io/badge/linkedin-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/pragti-chauhan-2132a61a2/)
