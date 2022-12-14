# GeoTAX Application for Kubernetes Deployment Guide
This guide provides detailed instructions for deploying the sample GeoTAX API in a Kubernetes environment.

## Install client tools
To deploy the GeoTAX application in a Kubernetes environment, install the following client tools that are applicable to your environment:
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
- [Helm 3](https://helm.sh/docs/intro/install/)
##### Amazon Elastic Kubernetes Service (EKS)
- [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)
- [eksctl](https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html)

## Deploy the GeoTAX application Docker image
The GeoTAX application is packaged as a Docker image and should be deployed to an accessible container registry, such as [Amazon ECR](https://docs.aws.amazon.com/AmazonECR/latest/userguide/Registries.html) for [EKS](https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html).

To build using the provided GeoTAX REST APIs, see [Sample GeoTAX Application for Docker](../docker/geotax/README.md)

## Create the Kubernetes cluster
The sample GeoTAX application requires a Kubernetes cluster with at least one node to run the GeoTAX application and a separate node for the NGINX ingress controller. This sample cluster will scale the number of nodes available for running the GeoTAX application up to a maximum of 5, based on user load.

##### Amazon EKS
>To create an Amazon EKS cluster, follow the instructions in [README.md](./cluster/eks/README.md). 

## Configure Helm
Add the required Helm chart repositories. These repositories will be used to deploy components in the cluster:
```
helm repo add stable https://charts.helm.sh/stable
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```
## Deploy the NGINX Ingress Controller and Prometheus-Adapter with Prometheus
   The GeoTAX application uses the NGINX Ingress Controller as a load balancer in order to monitor the number of active users. In addition,  Prometheus-Adapter is installed along with Prometheus Server in order to provide this data as custom metrics in Kubernetes, which the GeoTAX application uses to autoscale. 
   1. Install Prometheus Server using Helm:      
      ```
      helm install prometheus prometheus-community/prometheus
      ```
        
   2. Install Prometheus-Adapter using Helm:
      
      ```
      helm install prometheus-adapter prometheus-community/prometheus-adapter  -f ./gtx-ingress/prometheus-adapter/values.yaml
      ```
      **Note:** prometheus-adapter versions 3.0.1 to 3.0.3 don't honor Horizontal Pod Autoscaling. (For more details, please refer to this [issue](https://github.com/prometheus-community/helm-charts/issues/1739)). You can install specific version of prometheus-adapter for horizontal pod autoscaling feature using `--version` flag.  
   

   3. Install the NGINX Ingress Controller by executing this command:
        
      ```
      helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
      helm install nginx-ingress ingress-nginx/ingress-nginx -f ./gtx-ingress/nginx-ingress-controller/values.yaml 
      ```
      
      Wait for all pods to come up in a running state; it generally takes 2 to 5 minutes.
      ```
      kubectl get pods -w
      ```  
            
      Verify the installation of the NGINX Ingress controller using the following command. It may take few minutes for the external address to be available
         
      ```
      kubectl get --raw="/apis/external.metrics.k8s.io/v1beta1/namespaces/default/nginx_active_connections"
      ```
      - When the NGINX Ingress Controller is running properly, you will see the output below. You can now proceed to the next step.

        ```
        {"kind":"ExternalMetricValueList","apiVersion":"external.metrics.k8s.io/v1beta1","metadata":{"selfLink":"/apis/external.metrics.k8s.io/v1beta1/namespaces/default/nginx_active_connections"},"items":[{"metricName":"nginx_active_connections","metricLabels":{},"timestamp":"2020-04-28T10:48:44Z","value":"7"}]}
        ```       
      - If the command displays the following output message, it means Prometheus-Adapter is not running. Check its status and wait until it is up before retrying.
        >`Error from server (NotFound): the server could not find the requested resource`
              
      - If you see either of the following messages, it means the NGINX Ingress Controller is still coming online. You should wait a minute or two and try running the same command again.
        >`Error from server (NotFound): the server could not find the metric nginx_active_connections for namespaces`              
        
        >`Error from server (ServiceUnavailable): the server is currently unable to handle the request`

## Update credentials in Kubernetes secret

This is required to access data files from cloud storage. Place all credentials related information in the `./gtx/gtx-storage-secrets` folder.  Update the `./gtx/gtx-storage-secrets/rclone.conf` file with the appropriate configuration.  This file is already populated with sample configurations and placeholders for key information.  If there are supporting files needed for configuration, like service account JSON files, they should also be placed in this folder.  This folder will be mounted to the data preparation container at `/usr/local/gtx-storage-secrets`.

##### Amazon [S3](https://aws.amazon.com/s3/)

- Either update the example `rclone.conf` with your key information, or replace the `rclone.conf` with your own configured `rclone.conf` file.
    - `AWS_ACCESS_KEY_ID` - s3 access key
    - `AWS_SECRET_ACCESS_KEY`  - s3 secret key
    - `AWS_DEFAULT_REGION` - s3 region
    
After updating credentials, create the Kubernetes secret for the cluster:
```
kubectl create secret generic gtx-storage-secrets --from-file=./gtx/gtx-storage-secrets
``` 

## Configure the reference datasets
The GeoTAX application requires GeoTAX reference datasets, which are .spd files that must be available on [S3](https://aws.amazon.com/s3/) for EKS. The datasets will be accessed from the `./gtx/gtx-datasets-cm.yaml` config map.

* If you have not already downloaded the reference data, for information about Precisely's data portfolio, see the [Precisely Data Guide](https://dataguide.precisely.com/) where you can also sign up for a free account and access sample data available in [Precisely Data Experience](https://data.precisely.com/).

In the `./gtx/gtx-datasets-cm.yaml` file, specify the rclone path of each dataset file kept on cloud storage in the `spd.list` parameter.
          
Example using the AWS configuration:
```
  spd.list : |
    s3://com.pb.geotax/test/GPM062022.spd
```

Deploy the datasets manifest script:
```
kubectl apply -f ./gtx/gtx-datasets-cm.yaml
``` 

## Deploy the shared resources
These resources will be described the same across all Kubernetes platforms.

**Note:** Since the data files could either be zipped or unzipped, we have provided options in the `./gtx/gtx-dataprep-cm.yaml` file to execute the required code snippet and comment out the other. 

Run these commands:   
   ```  
   kubectl apply -f ./gtx/gtx-dataprep-cm.yaml 
   kubectl apply -f ./gtx/gtx-service.yaml
   kubectl apply -f ./gtx-ingress/gtx-ingress-resource.yaml 
   kubectl apply -f ./gtx/gtx-hpa.yaml  
   ```

## Deploy the GeoTAX application
GeoTAX SDK requires the reference data to be available on the file system of the pod running the GeoTAX service. Due to the size of the reference data, the data is managed outside of the docker image and configured during deployment. Two options for configuring the reference data are provided:

- Option A: The reference data is initialized on an [emptyDir volume](https://kubernetes.io/docs/concepts/storage/volumes/#emptydir)
- Option B: The reference data is initialized on a [persistent volume](https://kubernetes.io/docs/concepts/storage/volumes/#nfs)

#### Option A: Reference data is initialized on an emptyDir volume
This is the simplest approach to deploy the GeoTAX application. During startup, a GeoTAX pod copies the data from Cloud Storage (S3) to an emptyDir volume that's mounted to a local directory.

**Note**: Each new GeoTAX pod copies the data from the storage bucket to the local directory. This increases the pod startup time, so this approach may not be appropriate for production usage where faster startup time is preferred.

Steps to deploy:

  1. Add the GeoTAX application Docker image URI.
     
     For this, move to the `./gtx/local-data/gtx-runtime.yaml` file and replace:
     - `@IMAGE_URI@` - the URI of the GeoTAX application Docker image stored in the Docker repository in the `image` parameter. The `@IMAGE_URI@` parameter needs to be replaced in two places.
      ```
          initContainers:
             - name: gtx-dataprep-container
               image: @IMAGE_URI@
       ```
       and
       ```
          containers:
             - name: gtx-container
               image: @IMAGE_URI@
       ```  
 
  2. Deploy the GeoTAX application runtime: 
     
     ```
       kubectl apply -f ./gtx/local-data/gtx-runtime.yaml
     ``` 
#### Option B: Reference data is initialized on a persistent volume
This approach minimizes pod startup time by preparing the reference data ahead of deployment on a shared persistent volume.  The deployment of the GeoTAX data using a persistent volume is a 2-step process:

  1. Configure the persistent volume with reference data.
  2. Deploy the GeoTAX application using the data from the persistent volume.

 #### 1. Configure the persistent volume with reference data
 This sample demonstrates configuring a persistent volume backed by high performance cloud based file storage.  Though the steps outlined are written for specific products (`Amazon EFS`), the process is generally applicable for other persistent volume types as well. Follow the steps below based on your platform.
 ##### Amazon EKS
 >To deploy the GeoTAX reference data using [Amazon Elastic File System](https://aws.amazon.com/efs/), follow the instructions in [AmazonEKSReferenceData.md](gtx/nfs-data/AmazonEKSReferenceData.md).

#### 2. Deploy the GeoTAX application using the data from the persistent volume
The GeoTAX application uses the same persistent volume where you deployed the reference data in the previous step. 
 To deploy the application:
 
   - Deploy the persistent volume:
     
     **Note:** If you deployed the persistent volume in the previous step and have not deleted it, then it should be available. In that case, you can skip this step.
      ##### Amazon EKS
      ```
      kubectl apply -f ./gtx/nfs-data/eks/gtx-data-pv.yaml
      ```

   - In the `./gtx/nfs-data/gtx-runtime.yaml` file, replace:
     - `@IMAGE_URI@` - the URI of the Geotax application Docker image stored in the Docker repository in the `image` parameter.
   
   - Deploy the Geotax application runtime:
     ```
     kubectl apply -f ./gtx/nfs-data/gtx-runtime.yaml
     ``` 

**Note:**  For better performance in production usage, minimum number of replicas can be set to 2 or more as per the GeoTAX application requirements.

## Access the Geotax application
Once the above steps have completed, the Geotax application is up and running. You can access the Geotax services endpoints using a web browser. You can also use a Web Service invocation tool to access the REST service endpoints available in the application.

Retrieve the Ingress service address which exposes the Geotax application to the external world using this command:
              
  ```
  kubectl get services -o wide -w nginx-ingress-ingress-nginx-controller
   ```
Copy the External IP/URL and Port of the Ingress service, and then create the Geotax application test page URL: 
              
  `http://<External-IP>:<port>/`                
              
  For example: `http://ac97c5928849311ea9e8602781e57924-913734340.us-east-1.elb.amazonaws.com/`

