# Simple VPC Peering

Used to peer a central tools account (MetaServices) to child accounts (Dev, NonProd, Prod)

## Deploy child acceptor roles

> Note: I deployed the role via the master account so i didnt have to create additional cross account roles

Create the stackset
```
aws cloudformation create-stack-set --stack-set-name VPCPeeringSetup --template-body file://VPCPeerChildAcceptor.yaml --parameters file://VPCPeerChildAcceptor.json --capabilities "CAPABILITY_NAMED_IAM"
```

Add the child account
```
aws cloudformation create-stack-instances --stack-set-name VPCPeeringSetup --accounts "123456789012" "234567890123" "345678901234" --regions "ap-southeast-2"
```

(Optional) Update the stack instances if the template changes
```
aws cloudformation update-stack-set --stack-set-name VPCPeeringSetup --template-body file://VPCPeerChildAcceptor.yaml --parameters file://VPCPeerChildAcceptor.json --capabilities "CAPABILITY_NAMED_IAM"
```

## Deploy the Requestor stack

Deploy the requestor stack to Meta Services
```
aws cloudformation create-stack --stack-name VPCPeeringSetup --template-body file://VPCPeerParentRequestor.yaml --parameters file://VPCPeerParentRequestor.json --capabilities CAPABILITY_IAM
```