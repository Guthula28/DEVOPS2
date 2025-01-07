Yes, **Terraform** and **Ansible** need to be installed on your local machine (or CI/CD environment) for provisioning infrastructure and managing configuration, respectively. In the example below, I’ll guide you through installing Terraform and Ansible on an **AWS EC2 instance** running **Ubuntu**.

### **Steps to Install Terraform and Ansible on an AWS EC2 Instance (Ubuntu)**

#### **1. Launch an AWS EC2 Instance (Ubuntu)**

- Go to the AWS Management Console, select EC2, and launch a new instance with the **Ubuntu** AMI.
- Select a security group that allows SSH (port 22).
- Once the instance is up, connect via SSH:
  ```bash
  ssh -i "your-key.pem" ubuntu@your-ec2-public-ip
  ```

---

#### **2. Install Terraform on Ubuntu**

Follow these steps to install Terraform:

##### **2.1 Install Dependencies**
Update the package list and install `curl` and `unzip`:
```bash
sudo apt-get update
sudo apt-get install -y curl unzip
```
```
wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list

sudo apt update && sudo apt install terraform -y
```

##### **2.2 Verify Terraform Installation**
Check if Terraform is installed correctly:
```bash
terraform -version
```

You should see the installed Terraform version (e.g., `Terraform v1.6.0`).

---

#### **3. Install Ansible on Ubuntu**

##### **3.1 Update the System**
Before installing Ansible, update the system:
```bash
sudo apt-get update
```

##### **3.2 Install Ansible**
Install Ansible from the official Ubuntu repositories:
```bash
sudo apt-get install -y ansible
```

##### **3.3 Verify Ansible Installation**
Check if Ansible is installed correctly:
```bash
ansible --version
```

You should see the installed Ansible version (e.g., `ansible 2.9.x`).

##### ** Install AWS CLI

```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install unzip -y
unzip awscliv2.zip
sudo ./aws/install
```

##### Configure the AWS Credential i.e, access key and secret access key

```
aws configure

```
---
### Copy private key to your Project server

```
scp -i <key.pem> <key.pem> username@public_ip:
# login to instance and change permission for pem file
chmod 400 /home/ubuntu/project.pem

```
#### **4. Example Terraform + Ansible Setup**

Once both Terraform and Ansible are installed, you can use Terraform to provision infrastructure and Ansible to manage the configuration of those resources.

---

### **Example Project: Provision AWS EC2 Instance with Terraform and Configure it with Ansible**

#### **4.1 Terraform Configuration (main.tf)**

This Terraform configuration provisions an **EC2 instance** and then runs an **Ansible playbook** on it.

```hcl
provider "aws" {
  region = "ap-south-1"
}

# Define the security group
resource "aws_security_group" "web_sg" {
  name        = "web-sg"
  description = "Allow SSH and HTTP"

  ingress {
    description = "Allow SSH"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    description = "Allow HTTP"
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    description = "Allow all outbound traffic"
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = {
    Name = "Web Security Group"
  }
}

resource "aws_instance" "web" {
  ami           = "ami-0dee22c13ea7a9a67" # Ubuntu 20.04
  instance_type = "t2.micro"
  key_name      = "your-key"  # Replace with your EC2 key pair name
  security_groups = [aws_security_group.web_sg.name]

  tags = {
    Name = "Ansible-Terraform-EC2"
  }

  provisioner "local-exec" {
    command = "sleep 30 && ansible-playbook -i ${self.public_ip}, -u ubuntu --private-key /path/to/your/key ansible/playbook.yml"
  }
}

output "public_ip" {
  value = aws_instance.web.public_ip
}
```

#### **4.2 Ansible Playbook (playbook.yml)**

This playbook installs **Nginx** on the newly created EC2 instance.

```yaml
---
- hosts: all
  become: yes
  tasks:
    - name: Update apt packages
      apt:
        update_cache: yes

    - name: Install Nginx
      apt:
        name: nginx
        state: present

    - name: Start Nginx
      service:
        name: nginx
        state: started
        enabled: yes

    - name: Create a simple index.html
      copy:
        content: "<h1>Welcome to Devops Easy EC2 Instance</h1>"
        dest: /var/www/html/index.html
```

---

#### **5. Applying the Terraform and Ansible Configuration**

1. **Initialize Terraform**:
   ```bash
   terraform init
   ```

2. **Create an Execution Plan**:
   ```bash
   terraform plan
   ```

3. **Apply the Configuration**:
   ```bash
   terraform apply
   ```

This will:
- Provision an EC2 instance using **Terraform**.
- Use the **Ansible** playbook to install and configure **Nginx** on the instance after it’s created.

---

### **Verifying the Setup**

Once the setup is complete, you can access the **Nginx** web server via the public IP of the EC2 instance.

```bash
curl http://<your-ec2-instance-public-ip>
```

You should see the message: `"Welcome to Terraform-Ansible EC2 Instance"`.

---

### **Conclusion**

In this project, you’ve provisioned an EC2 instance using Terraform and configured it with Ansible. This combination of tools is powerful for automating infrastructure and configuration management.

Let me know if you need further clarifications or help with specific configurations!
