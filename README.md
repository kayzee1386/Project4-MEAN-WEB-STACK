# Project4-MEAN-WEB-STACK

### This Project covers the MEAN (MongoDB, Express, Angular and NodeJS) deployment on Ubuntu server.

Some little self learning were done on the following topics; OSI model, Load balancing and did some practice on HTML, CSS and Javascript.

### Step 1
NodeJs was install by first updating `sudo apt update` then, upgrading `sudo apt upgrade` and finally running an installation command `sudo apt install -y nodejs`.

![update](https://user-images.githubusercontent.com/46185705/132374060-e104ee16-cc78-4f97-b646-4b352b77acfc.jpg)
![upgrade](https://user-images.githubusercontent.com/46185705/132374078-67591523-1659-4ee5-98a5-c155d33d2b88.jpg)
![nodejs-installed](https://user-images.githubusercontent.com/46185705/132374111-b6c10952-1375-4862-b6ff-faba89a5f2e3.jpg)

### Step 2 (Mongo DB Installation)

MongoDB is store Data in flexible JSON-like documents. On this project, we added book records to MongoDB that contain info like book name, isbn number, author, and number of pages. And the following command were first ran before the installation command `sudo apt install -y mongodb`;

`sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6`
`echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list`

![mongo](https://user-images.githubusercontent.com/46185705/132377581-aaeb5053-7027-4fc1-8c73-4fb8008fe899.jpg)
![mongodb-installed](https://user-images.githubusercontent.com/46185705/132377593-4d32034b-925f-468b-8b12-b5020ce4eebd.jpg)

Afterwards, the server were started `sudo service mongodb start` and the service were verified `sudo systemctl status mongodb` to know if its up and running

![mongodb-status](https://user-images.githubusercontent.com/46185705/132380458-79c6e091-6c32-4bf2-bbd6-89f352056218.jpg)

Node Package manager was installed `sudo apt install -y npm`. Followed by a body parser which will help with the JSON file processing `sudo npm install body-parser`. In this directory 
