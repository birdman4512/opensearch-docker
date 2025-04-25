<a name="readme-top"></a>
# OpenSearch Docker Stack

A configured and working OpenSearch stack deployed with Docker. 

## Based on

* [Opensearch Docker](https://docs.opensearch.org/docs/latest/install-and-configure/install-opensearch/docker/)

<!-- ABOUT THE PROJECT -->
## About The Project

I wanted to build a good solid base OpenSearch cluster that could be used as a basis for other projects. 

The stack has features such as:
* Completely runs in Docker and so can be deployed anywhere docker can be run. 
* Has low resource requirements. It should be able to be deployed on machines with low memory. (2GB)
* Is configured automatically. No manual setup.
* Uses TLS encryption for all traffic between nodes.
* Has certificate based authentication enabled (OpenSearch Dashboards uses it to connect to the cluster.)
* Stores persistent data in a folder called `stack-data` which can be linked to a persistent, non operating system drive.


<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- GETTING STARTED -->
## Getting Started

The entire environment is deployed via Docker and heavily automated. 
 
### Prerequisites

Install Docker 
* https://docs.docker.com/engine/install/

Install docker-compose
* https://docs.docker.com/compose/install/


> [!NOTE]
> Especially on Linux, make sure your user has the [required permissions][linux-postinstall] to interact with the Docker
> daemon.

### Installation

1. Obtain the repo   
   ```sh
   # This can be done in a variety of ways depending on where it is being deployed. 
   # SCP the files to the box, use GIT to pull the repo. Adjust this step to what works best.
   
   cd /opt
   git clone https://github.com/birdman4512/opensearch-docker.git

   cd opensearch-docker
   ```
   
2. Change the max_map_count
   ```sh
   #Permanent  
   ## Alternatively from the command line run  
   echo 'vm.max_map_count=262144' > /etc/sysctl.conf

    #Reload the settings
   sudo sysctl -p
   ```

   Also see [Important System Settings][important-system-settings]
   
3. Create the .env file
   ```sh
   cp docker-compose.env .env
   ```
   
4. Edit the .env file and set a strong password for `GLOBAL_ADMIN_PASS`. Review other settings for any necessary changes, comments explain what can be done. 
   ```sh
   vi .env
   ```
    
5. Initialize the stack and setup security / users
   ```sh 
   docker-compose run setup
   ```
   
6. Bring up the Environment
   ```sh
   docker-compose up
   ```
   
> [!NOTE]
> You can also run all services in the background (detached mode) by appending the `-d` flag to the above command.

## Stack Backup

All persistent data for the stack is stored within the `stack-data` folder. This folder can be

* Symbolic linked to a second drive to store cluster data off the Sys drive. 
* Backed up to move the cluster to another host. 

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Troubleshooting

If you experience issues setting up the stack, you can generally re-run the setup process to correct any issues including expiring certificates. 

```sh
#Navigate to the correct folder.
cd /opt/opensearch-docker

#Bring down the clustomer
docker-compose down

#Re-run setup
docker-compose run setup
```

[EXPERT] If you would like to make changes to the Security settings for OpenSearch:
* Make the necessary adjustments in the files in `config\setup\config`. 
> [!NOTE]
> The default security settings creates an account `admin` with a low security password of `admin`.
> During the setup, this password is updated to the one provided in the `.env` file.
> To ensure that your cluster does not have a low security password on the admin account, ensure you remove both files mentioned below.

* Remove the the following files
    * `stack-data\certificates\setup_opensearch.txt` - Will re-run the securityadmin.sh script, re-importing settings into the cluster. 
    * `stack-data\certificates\setup_users.txt` - Will re-set account passwords from the default set in the securityadmin.sh setup. 

* Re-run the setup container
```sh
docker-compose run setup
```

* Restart the stack
```sh
docker-compose up -d
```

<!-- LICENSE -->

## License

Distributed under the MIT License. See `LICENSE` for more information.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- CONTACT -->

## Contact

Dean B

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[linux-postinstall]: https://docs.docker.com/engine/install/linux-postinstall/
[important-system-settings]: https://docs.opensearch.org/docs/latest/install-and-configure/install-opensearch/index/#important-settings