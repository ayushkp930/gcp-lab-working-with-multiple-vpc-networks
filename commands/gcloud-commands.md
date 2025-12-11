# Commands used (Cloud Shell)

# Create VPC
gcloud compute networks create privatenet --subnet-mode=custom

# Create subnets
gcloud compute networks subnets create privatesubnet-us --network=privatenet --region=us-central1 --range=172.16.0.0/24
gcloud compute networks subnets create privatesubnet-notus --network=privatenet --region=europe-west1 --range=172.20.0.0/20

# Create firewall
gcloud compute firewall-rules create privatenet-allow-icmp-ssh-rdp --network=privatenet --direction=INGRESS --priority=1000 --action=ALLOW --rules=icmp,tcp:22,tcp:3389 --source-ranges=0.0.0.0/0

# Create VM
gcloud compute instances create privatenet-us-vm --zone=us-central1-c --machine-type=e2-medium --subnet=privatesubnet-us
