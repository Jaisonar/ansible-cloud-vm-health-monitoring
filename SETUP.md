# Setup & Execution - Ansible Cloud VM Health Monitoring

This guide provides step-by-step instructions to set up the environment, configure AWS, and run the Ansible VM health monitoring playbooks.

1. Update the System
sudo apt update && sudo apt upgrade -y

2. Add the Ansible PPA
sudo add-apt-repository --yes --update ppa:ansible/ansible

3. Install Ansible
sudo apt install ansible -y

4. Install AWS CLI
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install unzip -y
unzip awscliv2.zip
sudo ./aws/install
aws configure

5. Tagging Script
#!/bin/bash

# Fetch instance IDs with Environment=dev and Role=web
instance_ids=$(aws ec2 describe-instances \
  --filters "Name=tag:Environment,Values=dev" "Name=instance-state-name,Values=running" \
  --query 'Reservations[*].Instances[*].InstanceId' \
  --output text)

# Sort instance IDs
sorted_ids=($(echo "$instance_ids" | tr '\t' '\n' | sort))

# Rename instances sequentially
counter=1
for id in "${sorted_ids[@]}"; do
  name="web-$(printf "%02d" $counter)"
  echo "Tagging $id as $name"
  aws ec2 create-tags --resources "$id" \
    --tags Key=Name,Value="$name"
  ((counter++))
done

Make it executable:

chmod +x tag_instances.sh
./tag_instances.sh

6. Python Virtual Environment & Packages
sudo apt install python3-venv -y
python3 -m venv ansible-env
source ansible-env/bin/activate
pip install boto3 botocore
ansible-galaxy collection install amazon.aws

7. Dynamic Inventory Validation
ansible-inventory -i inventory/aws_ec2.yaml --graph

8. Copy SSH Public Key to EC2 Instances

Create copy_ssh_key.sh:

#!/bin/bash

PEM_FILE="DevOps-Shack.pem"
PUB_KEY=$(cat ~/.ssh/id_rsa.pub)
USER="ubuntu"  # or ec2-user
INVENTORY_FILE="inventory/aws_ec2.yaml"

HOSTS=$(ansible-inventory -i $INVENTORY_FILE --list | jq -r '._meta.hostvars | keys[]')

for HOST in $HOSTS; do
  echo "Injecting key into $HOST"
  ssh -o StrictHostKeyChecking=no -i $PEM_FILE $USER@$HOST "
    mkdir -p ~/.ssh && \
    echo \"$PUB_KEY\" >> ~/.ssh/authorized_keys && \
    chmod 700 ~/.ssh && chmod 600 ~/.ssh/authorized_keys
  "
done

Make it executable:

chmod +x copy_ssh_key.sh
./copy_ssh_key.sh

9. Run the Ansible Playbook
ansible-playbook playbook.yaml

## Clone the Repository

Clone this repository to your local machine:

git clone https://github.com/Jaisonar/ansible-cloud-vm-health-monitoring.git
cd ansible-cloud-vm-health-monitoring
