# Omniverse_SDG_Sizing

### 1. **Verify GPU Driver Version**
Ensure your GPU driver version is 535.129.03 or later by running the following command:  
```sh
nvidia-smi
```  
### 2. **Install NVIDIA Container Toolkit**  
To set up the NVIDIA Container Toolkit, follow the instructions in the [installation guide](https://docs.omniverse.nvidia.com/isaacsim/latest/installation/install_container.html).   
Once installed, restart Docker:
```sh
sudo systemctl restart docker
```

### 3. **Clone the Repository**  
Download the project repository by running:  
```sh
git clone https://github.com/leekunhan/omniverse_sdg_sizing.git
```   

### 4. **Navigate to the Project Directory**
Change to the project directory:  
```sh
cd omniverse_sdg_sizing
```  

### 5. **Set Up NGC API Key and Log In**  
Follow the steps in the [Generate Your NGC API Key](https://docs.nvidia.com/ngc/gpu-cloud/ngc-user-guide/index.html#generating-api-key) guide to create an API key. Log in to NGC via the command line to download the Isaac Sim container:
 
```sh
docker login nvcr.io
 Username: $oauthtoken
 Password: 
```
> Note: Your password will be stored unencrypted in /home/username/.docker/config.json. To avoid this, configure a credential helper as recommended in the Docker documentation.
### 6. **Download the Isaac Sim Container**
Pull the Isaac Sim container image:  
```sh
docker pull nvcr.io/nvidia/isaac-sim:4.1.0
```  
> Note: The first pull may take 5-10 minutes.  

### 7. **Run the Isaac Sim Container**  
Start the Isaac Sim container with the following command:  
```sh
docker run --name isaac-sim --entrypoint bash \
-it --runtime=nvidia --gpus all \
-e "ACCEPT_EULA=Y" --rm --network=host \
-e "PRIVACY_CONSENT=Y" \
-v ~/docker/isaac-sim/cache/kit:/isaac-sim/kit/cache:rw \
-v ~/docker/isaac-sim/cache/ov:/root/.cache/ov:rw \
-v ~/docker/isaac-sim/cache/pip:/root/.cache/pip:rw \
-v ~/docker/isaac-sim/cache/glcache:/root/.cache/nvidia/GLCache:rw \
-v ~/docker/isaac-sim/cache/computecache:/root/.nv/ComputeCache:rw \
-v ~/docker/isaac-sim/logs:/root/.nvidia-omniverse/logs:rw \
-v ~/docker/isaac-sim/data:/root/.local/share/ov/data:rw \
-v ~/docker/isaac-sim/documents:/root/Documents:rw \
-v $PWD/sdg_sizing:/isaac-sim/sdg_sizing:rw \
-v $PWD/replicator_data/:/isaac-sim/replicator_data:rw \
nvcr.io/nvidia/isaac-sim:4.1.0
```  
> **Note:**  
> * The -e `ACCEPT_EULA=Y` flag indicates your acceptance of the NVIDIA Omniverse License Agreement.  
> * The -e `PRIVACY_CONSENT=Y` flag signifies your consent to data collection as outlined in the Omniverse Data Collection & Use FAQ. You may opt out by omitting this flag.  
> * Optionally, you can set the -e `PRIVACY_USERID=` flag to tag session logs with a specific user ID.

### 8. **Set Execution Permissions for Scripts**  
Ensure that the scripts within the container are executable:  
```sh
chmod +x -R sdg_sizing/
```  
## 9. **Execute the Benchmark Scripts**  
To run the dynamic SDG sizing benchmark:  
```sh
./sdg_sizing/dynamic_sizing/dynamic_data_benchmark_sdg.sh
```  
To run the statistical SDG sizing benchmark:  
```sh
./sdg_sizing/statistic_sizing/statistics_data_benchmark_sdg.sh
```  