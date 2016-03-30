# Setup Ubuntu Server in Amazon EC2

In this tutorial you will learn how to create and set up a Ubuntu Server for use with Bons' environment

## Creation

### Step 1
  Go to [EC2 Management Console](https://console.aws.amazon.com/ec2/v2/home) go to Instances -> Launch Instance
  [photo]

### Step 2
  Select the Ubuntu Server in this case we will use Ubuntu Server 14.04
  [photo]
  Select the instance type, instance details, storage

### Step 3
  Always add a tag name
  [photo]

  Configure the security group, make sure to open the ssh port (22) and the port of your Nodejs process (in Bons we use `8080`)
  [photo]

### Step 4
  Create and download a key pair for ssh connections
  [photo]

Now we have the new instance of the server done. You can connect via ssh to the server using this command in your terminal
```bash
ssh -i mypair.pem ubuntu@myserverip
```

_NOTE: replace __mypair.pem__ with the path of the file you download in step 4, replace __myserverip__ with the ip of the server_

## Set Up

Connect via ssh to the server.
You need to add your ssh public key in to authorized keys of the user ubuntu. (see [how to create a ssh key](./how-to-create-ssh-key.md))

Edit the file __~/.ssh/authorized_keys__:
```bash
nano ~/.ssh/authorized_keys
```

Paste the content of your __id_rsa.pub__ on the last line. Type ctrl-x to exit and save the file.
Now you can connect to the server without the amazon pem using `ssh ubuntu@myserverip`

### Automatically
Download and run the script:
```bash
curl -s https://example.com/script.sh | source /dev/stdin
```

### Manually

#### Step 1
We need to install the last version of Nodejs:
```bash
curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
sudo apt-get install -y nodejs
```

Install Gulp:
```bash
sudo npm i -g gulp-cli
```

Install Foreverjs:
```bash
sudo npm i -g forever
```

#### Step 2
Install MongoDB, if your project requires.

Import the public key used by the package management system.
```bash
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
```
Create a list file for MongoDB
```bash
echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.0.list
```

Reload local package database
```bash
sudo apt-get update
```

Install MongoDB:
```bash
sudo apt-get install -y mongodb-org
```

#### Step 3
TODO create user