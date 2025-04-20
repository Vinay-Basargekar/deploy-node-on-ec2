# Deploying a Node.js Application on AWS EC2

This guide provides step-by-step instructions for deploying a simple Node.js application on an AWS EC2 instance.

## Option 1: Direct Setup on EC2

### 1. Create EC2 Instance
- Launch an Ubuntu EC2 instance from the AWS Management Console
- Configure security group to allow inbound TCP traffic on port 3000 from anywhere

### 2. Connect to the Instance
SSH into your EC2 instance.

### 3. Install Node.js and Create Application
```bash
# Update package lists and install nodejs and npm
sudo apt update
sudo apt install nodejs npm -y

# Create project directory
mkdir my-node-app
cd my-node-app

# Initialize npm project
npm init -y

# Create server file
nano index.js
```

### 4. Add the following code to index.js
```javascript
const http = require('http');
const server = http.createServer((req, res) => {
  res.end("Hello from EC2 Node.js Server 🚀");
});
const PORT = 3000;
server.listen(PORT, () => {
  console.log(`Server is running on http://localhost:${PORT}`);
});
```

### 5. Run the Application
```bash
node index.js
```

### 6. Access Your Application
Open a web browser and navigate to:
```
http://<your-ec2-public-ip>:3000
```

## Option 2: Develop Locally and Deploy to EC2

### 1. Set Up Project Locally
Create the following folder structure:
```
my-node-app/
├── index.js
└── package.json
```

#### index.js
```javascript
const http = require("http");
const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader("Content-Type", "text/plain");
  res.end("Hello from EC2 Node.js Server 🚀");
});
server.listen(3000, "0.0.0.0", () => {
  console.log("Server running on http://0.0.0.0:3000");
});
```

#### Create package.json
```bash
cd my-node-app
npm init -y
```

### 2. Connect to Ubuntu EC2 from Local Terminal
```bash
ssh -i "your-key.pem" ubuntu@<YOUR_EC2_PUBLIC_IP>
```

### 3. Install Node.js on EC2 Ubuntu
Once connected to EC2 terminal:
```bash
sudo apt update
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs
```

### 4. Copy Local Project to EC2
On your local machine terminal:
```bash
scp -i "your-key.pem" -r ./my-node-app ubuntu@<YOUR_EC2_PUBLIC_IP>:/home/ubuntu/
```

### 5. Run Your Node App on EC2
Back in the EC2 terminal:
```bash
cd my-node-app
node index.js
```

### 6. Access Your Application
Open a web browser and navigate to:
```
http://<YOUR_EC2_PUBLIC_IP>:3000
```

## Troubleshooting

- If you cannot connect to your application, verify that:
  - The EC2 security group allows inbound traffic on port 3000
  - Your application is running without errors
  - You're using the correct public IP address

- For connection issues, check EC2 instance status and security group rules
- For application errors, examine the console output from your Node.js application