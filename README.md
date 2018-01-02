# Description

## Preview
`gcloud deployment-manager deployments create nataas-v1 --config NATaas.yaml --preview`

## Deploy
`gcloud deployment-manager deployments create nataas-v1 --config NATaas.yaml`

## Test it

Add two VMs with no external IP (can't communicate with internet)
`gcloud compute instances create vm1 vm2 --no-address`

Create third VM to use as jumphost to access internal VMs
`gcloud compute create vm3`

Add NAT tag to vm2
`gcloud compute instances add-tags vm1 --tags no-ip`

### Connect to natted VM
SSH to vm3
`gcloud beta compute ssh vm3`

SSH to vm1 from vm3
`gcloud beta compute ssh vm1 --internal-ip`

Test connection works
`ping 8.8.8.8`

### Connect to unnatted VM
SSH to vm3
`gcloud beta compute ssh vm3`

SSH to vm2 from vm3
`gcloud beta compute ssh vm1 --internal-ip`

Test connection does not work
`ping 8.8.8.8`

## Remove NAT tag
`gcloud compute instances remove-tags vm1 --tags=no-ip`

## Undeploy
`gcloud deployment-manager deployments delete nataas-v1`


## Links
 - [Overview of API Endpoints](https://cloud.google.com/deployment-manager/docs/configuration/supported-resource-types)