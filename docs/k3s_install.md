

## Prerequisites
should be noted you should install the firewall before the 
`sudo systemctl disable ufw.service && ufw disable`

## The system so far

honey1 = ubuntu 24.04.2 LTS
honey2 = ubuntu 24.04.2 LTS
honey3 = ubuntu 24.04.2 LTS

## K3s resources
https://docs.k3s.io/installation

https://docs.k3s.io/cluster-accessP

# 1 ssh'in 
pretty simple all ou do is ssh into the nodes i just opened 3 terminals and had them diplayed at the same time


# 2 Putting k3s in the honey 

First i used the script to install the first main service node on honey1 for the controlplane node
 `curl -sfL https://get.k3s.io | sh -`

this creates a token in the etc/rancher/k3s/k3s.yaml that you will need to copy for later 

Then it was a matter of installing the worker planes
`curl -sfL https://get.k3s.io | K3S_URL=https://myserver:6443 K3S_TOKEN=mynodetoken sh -`

#note funny thing you are supposed to replace the "my server" with the ip of the controlplane aka the first k3s node you get running i put the ip of the node i was installing it on at first - dont do that

it should take a few seconds, if it dose not complete quickly check your ip addresses, it should not take longer then 5 mins to install

# kubectl on host machine 
remeber the token that you copied, you need that whole file so copy it 

then on your host we need to create the place for it to live

`mkdir ~/.kube/config` then paste the contents of the previous file onto the newly added config 




