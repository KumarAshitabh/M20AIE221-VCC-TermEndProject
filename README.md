### 1. Brief Introduction
This is a fog computing application developed in Python using Twisted networking framework and Celery task queue.  
As the Internet of Things technology develops, more and more smart devices are connected to the core network, generating a large amount of data every day. Using cloud computing to deal with this kind of data has led to two major issues: network congestion and high latency.  
In order to solve this problem, a new computing model was proposed, that is, fog computing. The main idea of fog computing is to deploy some servers near the users to provide service with low latency. These servers are also called fog nodes.  
When a fog node receives a lightweight task, it can process the task locally. When a fog node receives a middleweight task, it can collaborate with neighbour fog nodes to process the task. When a fog node receives a heavyweight task, it can upload the task to the cloud.  

The above figure shows the basic architecture of fog computing. This project can be divided into three parts: fog node application, cloud application and user application. User application can send tasks to fog nodes with different speeds, fog node application can process, offload or upload the tasks, cloud application can process the task and send the results back to fog nodes.

### 2. Deployment and Installation
The basic requirements for this project include Ubuntu operating system for fog nodes, AWS EC2 /Azure VM instance with Ubuntu operating system for cloud server and a smart device that supports Python3 script execution (e.g. an Android phone or Android emulator with Python3 IDE).

#### 2.1 Deployment of fog node
The first step is the preparation of the Ubuntu OS for the fog node application. It is recommended to use VirtualBox to create a virtual machine with Ubuntu, because if you have successfully configure one fog node in a virtual machine, you can simply duplicate the virtual machine to create many virtual fog nodes quickly in one computer. If you use VirtualBox, it is important to set the network adapter of the virtual machines to `bridge mode` which can assign different IP addresses to different virtual machines.  
The necessary software and libraries required for fog node include Git, Pip3, Redis, Twisted and Celery. You can use terminal to finish the installation.  
Install git.
```javascript
sudo apt-get update
sudo apt-get install git
```
Install pip3.
```javascript
sudo apt-get install python3-pip
```
Install Redis database.
```javascript
sudo apt-get install redis-server
```
Install Redis Python library.
```javascript
pip3 install redis
```
Install Twisted networking framework.
```javascript
pip3 install twisted
```
Install Celery task queue.
```javascript
pip3 install Celery
```

#### 2.2 Deployment of Cloud server
VM - Ubuntu 20.04  
```javascript
sudo apt-get update
sudo apt-get install git
sudo apt-get install python3-pip
sudo apt-get install redis-server
pip3 install redis
pip3 install twisted
pip3 install Celery
```

### 3. Start Service
Execute the cloud application, fog node application and user application to make it work. Remember that cloud application should be started first, then fog node application, finally user application.

#### 3.1 Start Cloud Application
Cloud application should be started first, so that fog nodes can connect to cloud server when they are started.  

```
Use a new terminal window (connected to the cloud), launch Redis database.
```javascript
redis-server
```
Use a new terminal window (connected to the cloud), launch Celery task queue.
```javascript
cd FogComputing
celery -A tasks worker --loglevel=info --concurrency=1
```
Use a new terminal window (connected to the cloud), launch the cloud application. The cloud application is `cloud_server_simplified.py.py`.
```javascript
cd FogComputing
python3 cloud_server_simplified.py
```

#### 3.2 Start Fog Node Application
The fog nodes running in the same LAN can collaborate with each other.  
Clone this project to the fog node virtual machine.
```javascript

```
Open a new terminal window, launch Redis database. Redis is used as the broker and backend of Celery task queue.
```javascript
redis-server
```
Open a new terminal window, launch Celery task queue. `tasks` is the Python file where you define your tasks.
```javascript
cd FogComputing
celery -A tasks worker --loglevel=info --concurrency=1
```
Open a new terminal window, launch the fog node application. The fog node application is `server.py`.
```javascript
cd FogComputing
python3 server.py
```

#### 3.3 Start User Application
Execute the `client_for_phone.py` in your mobile phone with Python IDE.  Pydroid 3 https://play.google.com/store/apps/details?id=ru.iiec.pydroid3&hl=en_IN&gl=US

client = Client('192.168.1.9', 10000) # Fog node 

```javascript
cd FogComputing
python3 client_for_phone.py
```


References:
https://github.com/Dongfeng-He/FogComputing





