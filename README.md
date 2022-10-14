# Haqq. False alarm!

I'm going to use TenderDuty v2 for my node monitoring. 

With the help of TenderDuty v2 you can monitor nodes, check network height, validator status, uptime. It is also possible to connect notifications to Telegram and Discord.

We have to update the repositories

	sudo apt update && sudo apt upgrade -y
	

Install the necessary utilities
	
	sudo apt install curl build-essential git wget jq make gcc tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y

Install docker 
	
	apt install apt-transport-https ca-certificates curl software-properties-common -y && \
	curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - && \
	add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable" && \
	apt update && \
	apt-cache policy docker-ce && \
	sudo apt install docker-ce -y && \
	docker --version

**Install tenderduty**

Open a new session in the screen
	
	screen -S tenderduty
	
	cd ~
	mkdir -p tenderduty && cd tenderduty
	docker run --rm ghcr.io/blockpane/tenderduty:latest -example-config >config.yml

Now we can chack config with a command 

	nano $HOME/tenderduty/config.yml
	


**To set up monitoring we need to change in the config:**

_name of the network_

	sed -i 's/Osmosis/haqq/' ~/tenderduty/config.yml

_chain-id_

	sed -i 's/osmosis-1/haqq_54211-2/' ~/tenderduty/config.yml
	
_valoper_address_

	sed -i 's/osmovaloper1xxxxxxx.../haqqvaloper1zqdc83g38lcmcn3pr7c5p5wkz9wf4lqzefejgk/' ~/tenderduty/config.yml

Then we need to start docker 

	docker run -d --name tenderduty -p "8888:8888" -p "28686:28686" --restart unless-stopped -v $(pwd)/config.yml:/var/lib/tenderduty/config.yml ghcr.io/blockpane/tenderduty:latest

Checking logs 

	docker logs -f --tail 20 tenderduty

Now we can check the information in the browser by going to the address 

	http://your_ip:8888/

We are done with installing monitoring system!
