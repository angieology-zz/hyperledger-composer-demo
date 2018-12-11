
# Hyperledger Composer + Fabric Demo

---
_“Only those who have the patience to do simple things perfectly will we acquire the skill to do difficult things easily” - Friedrich Schiller_

---

## Steps 🐝

- Set up google compute engine
- Install prerequisites
- start fabric network
- create business logic with composer
- install composer logic to network
- generate REST API

---

<img src="https://hyperledger.github.io/composer/v0.19/assets/img/Composer-Diagram.svg">

---

## Compute Engine Setup 🦕

cloud.google.com 


create a compute engine with

1. 2 cores
2. Ubuntu 16 boot disk
3. Allow full access to all cloud APIs
4. Allow HTTP traffic
5. Allow HTTPS traffic
---
clone this repo

```
git clone https://github.com/angieology/hyperledger-composer-demo.git
&& cd hyperledger-composer-demo

chmod u+x install-prereqs.sh

./install-prereqs.sh
```

log out
log back in

---
# Composer Playground
composer-playground.mybluemix.net
export bna file

---
Now we have to import the bna file into our VM

Create a storage bucket
- name: whatever you want
- default everything

upload my-basic-sample.bna file to bucket
```
gsutil cp gs://nobul-hlf-artifacts-test/my-basic-sample.bna .
```

---
# start Fabric
```
cd ~/fabric-dev-servers

export FABRIC_VERSION=hlfv11

./downloadFabric.sh 

./startFabric.sh

# every network needs an admin
./createPeerAdminCard.sh
```

---

# start Composer network
```
cd ~/hyperledger-demo

composer network install 
	--card PeerAdmin@hlfv1 
	--archiveFile my-basic-sample.bna

composer network start 
	--networkName my-basic-sample
	--networkVersion {{ network version }}
	--networkAdmin admin 
	--networkAdminEnrollSecret adminpw 
	--card PeerAdmin@hlfv1 
	--file networkadmin.card
```
    

---

# add card to network
card is the bundle of our custom code
```
composer card import --file networkadmin.card

# test card is properly installed
composer network ping --card admin@my-basic-sample
```
---
# start composer rest API
```
composer-rest-server 
	-c admin@my-basic-sample
	-n always 
	-u true 
	-w true
```

Composer Rest Server is now running on port 3000
