# How to Setup docker:

# Install docker:
	https://docs.docker.com/engine/install/ubuntu/
	
# Install docker Machine
	https://docs.docker.com/machine/install-machine/

<<<<<<< HEAD
# Install docker Compose
	https://docs.docker.com/compose/install/
	
# Find Images:
	hub.docker.com
	
# Start a docker container

		docker container run --publish 80:80 "image"
		
		docker container run --publish 80:80 --name webhost "image"
		
		docker container run --publish 80:80 --detach --name webhost "image"
		
		docker container run --publish 80:80 --detach --name webhost --env "env string" "image"
		
    docker container run --publish 3306:3306 --detach --name db -e MYSQL_RANDOM_ROOT_PASSWORD=yes mysql
			
		docker container run --publish 80:80 --detach --name "name" --network "network name" "image"


# Stop a docker container
		docker container stop "name" or "id"
	
# Show running containers
		docker container ps

# Show all containers
		docker container ls -a
	
# Check stats of a container

		show processes running in container
      # docker container top "name"
		
		shows the whole config of the container
      # docker container inspect "name"
		
		shows the running stats of the container
		  # docker container stats "name"

		
	
	
# DOCKER NETWORKING:
	
	- All containers us the "Bridge" network by default
	- All containers on the same virtual network can talk to each other
	- you can create new virtual networks
	- Continers can be attached to more than one network at a time
	
		
		This will show you the current IP of the container on the virtual network
      # docker container inspect --format '{{ .NetworkSettings.IPAddress }}' "name"
					
		Show what port the container is using
		  # docker container port "name"

		
		Show the docker networks
      # docker network ls
			
		  # docker network inspect
		
		  # docker network create --driver
		
		  # docker network connect
		
		  # docker network disconnect
	
	- To move a container to a new network, use the "connect" command and then the "disconnect" command
		  # docker network connect "name of new network" "name of the container"
		  # docker network disconnect "name of old network" "name of the container"		
		  # docker network connect bridge new_nginx
		  # docker network disconnect my_app_net new_nginx
		
	
# DOCKER DNS

	- by default the bridge network has a DNS service running
	- you can connect between containers using their unique names
	
	Multiple containers can respond to the same DNS name (Round Robin):
		
		Create a new network (or use an existing one)
			# docker network create "name"
		
		Create 2 new containers
		  # docker container run --detach --name "name1" --network "name" --net-alias "name" "image"
		  # docker container run --detach --name "name2" --network "name" --net-alias "name" "image"
		
	  NSlookup
	    # docker container run --rm --net dns alpine nslookup search
		
		This will spin up a container using alpine and run an NSlookup for "search" and provide the results, then spin down the container:			
			# docker container run --rm --net dns alpine nslookup search
									Unable to find image 'alpine:latest' locally
									latest: Pulling from library/alpine
									540db60ca938: Pull complete 
									Digest: sha256:69e70a79f2d41ab5d637de98c1e0b055206ba40a8145e7bddb55ccc04e13cf8f
									Status: Downloaded newer image for alpine:latest
									Server:         127.0.0.11
									Address:        127.0.0.11:53

									Non-authoritative answer:
									*** Can't find search: No answer

									Non-authoritative answer:
									Name:   search
									Address: 172.19.0.3
									Name:   search
									Address: 172.19.0.2
		
		This will, spin up a container using centos, and run a curl for search on port 9200, it will provide the resutls, then spin down the container:
			
			# docker container run --rm --net dns centos curl -s search:9200
								{
								  "name" : "Radian",
								  "cluster_name" : "elasticsearch",
								  "cluster_uuid" : "bovW6IR4QBym1pW_XXEKUQ",
								  "version" : {
									"number" : "2.4.6",
									"build_hash" : "5376dca9f70f3abef96a77f4bb22720ace8240fd",
									"build_timestamp" : "2017-07-18T12:17:44Z",
									"build_snapshot" : false,
									"lucene_version" : "5.5.4"
								  },
								  "tagline" : "You Know, for Search"
								}
			# docker container run --rm --net dns centos curl -s search:9200
								{
								  "name" : "John Sublime",
								  "cluster_name" : "elasticsearch",
								  "cluster_uuid" : "cmNybEjSQhOKIvGgQydzMA",
								  "version" : {
									"number" : "2.4.6",
									"build_hash" : "5376dca9f70f3abef96a77f4bb22720ace8240fd",
									"build_timestamp" : "2017-07-18T12:17:44Z",
									"build_snapshot" : false,
									"lucene_version" : "5.5.4"
								  },
								  "tagline" : "You Know, for Search"
								  

# STORAGE		
	Persistent Data
	
	- Data Volumes
	-	Makes a special location out side of the container UFS
	-	Removing a container, does not remove a volume
		
		Named Volumes:
		You can specify a name for the volume
			
			# docker container run -d --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=True -v "new volume name: mysql-db:/var/lib/mysql" "Image"
			
			# docker volume ls 
					DRIVER    VOLUME NAME
					local     0392694898ddf274170cfba98547222fb49564346646a7e96dcc4b2e6c4ea32f
					local     mysql-db
					
			
		
		
	
	Bind Mounts
		Link container path to host path		
		 # docker container run -d --name "name" -e MYSQL_ALLOW_EMPTY_PASSWORD=True -v /user/phenom/mysql:/var/lib/mysql "image"
		
	
		
# DOCKER COMPOSE:

	Two parts
		YAML File
			Containers, Networks, Volumes
		CLI Tool
			Docker-Compose
			
	Sample Yaml File:
	
		version: '3.1'  # if no version is specified then v1 is assumed. Recommend v2 minimum

		services:  # containers. same as docker run
		  servicename: # a friendly name. this is also DNS name inside network
			image: # Optional if you use build:
			command: # Optional, replace the default CMD specified by the image
			environment: # Optional, same as -e in docker run
			volumes: # Optional, same as -v in docker run
		  servicename2:

		volumes: # Optional, same as docker volume create

		networks: # Optional, same as docker network create
	
	
		
	CLI TOOL:
	
		Commands:
		
		setup volumes/networks and start all containers
		  # docker-compose up

    stop all containers and remove cont/vol/networks 	
      # docker-compose down
				
	  This will "detach" the containers, run in the background
			# docker-compose up -d
				
		this will show the running logs of all containers		
			# docker-compose logs

# env file
=======
env file
>>>>>>> b9d2904ddc9cea62544451b4df11080363692df5

    Commands:

      Specify a env file location:
        # docker-compose --env-file ~/vm/service/.env up -d



# Hash Password

     -  Docker Compoes doesn't like plan text passwords.. so hash it:

        #  echo $(htpasswd -nb <USER> <PASSWORD>) | sed -e s/\\$/\\$\\$/g