# helm-azure-deploy
# Repository README

This repository contains the source code and configuration files for deploying a Helm chart with dynamic deployment, service, and ingress templates. The repository includes an Azure Pipeline configuration file (`azure-pipeline.yaml`) and a Helm chart inside helm folder (`k8s-chart`) with a `values.yaml` file and templates for deployment, service, ingress and autoscaling.

## Azure Pipeline Configuration

The `azure-pipeline.yaml` file defines the build and deployment pipeline for the application. It consists of the following stages:

### Build

The `Build` stage is responsible for building the Docker image and pushing it to a container registry. It performs the following tasks:

1. Uses the Docker task to build, push, and tag the Docker image based on the specified Dockerfile.
2. Publishes the Helm chart artifact by copying the chart to the pipeline workspace.

### Generate values.yaml

The `Generate values.yaml` stage updates the `values.yaml` file with the necessary configuration for the Helm chart deployment. It performs the following tasks:

1. Installs the required PowerShell module for working with YAML.
2. Reads the original `values.yaml` file and converts it to a PowerShell object.
3. Updates the `image` property in the PowerShell object with the dynamically generated image path.
4. Converts the updated PowerShell object back to YAML format.
5. Overwrites the original `values.yaml` file with the updated version.
6. Publishes the updated `values.yaml` file as a separate artifact.

## Helm Chart

The `helm-chart` directory contains the Helm chart files for deploying the application. The `values.yaml` file in this directory is where you define the configuration parameters for your application. The following sections describe the important sections in the `values.yaml` file:

### metadata

The `metadata` section allows you to specify metadata for your Helm release. You can set the name of the release and the image name for the application. The image value will be dynamically updated during the pipeline execution.

### service

The `service` section defines the configuration for the Kubernetes service associated with the application. You can specify the service type, such as LoadBalancer, NodePort, or ClusterIP.

### resources

The `resources` section allows you to set resource limits (CPU and memory) for your application. You can define the limits and requests for both CPU and memory.

### ingress

The `ingress` section is used to configure an Ingress resource for your application. You can enable or disable the ingress, specify annotations, and define other ingress-related settings.

### autoscaling

The `autoscaling` section enables or disables the autoscaling feature for your application. You can set the minimum and maximum number of replicas, as well as the target CPU and memory thresholds for autoscaling.

## Dynamic Templates

The Helm chart includes dynamic templates for the deployment, service, and ingress resources. These templates are automatically generated based on the values provided in the `values.yaml` file. To customize the deployment, service, or ingress, you simply need to modify the corresponding values in the `values.yaml` file. During the pipeline execution, the updated values will be used to generate the appropriate Kubernetes manifests.

Please note that you should only modify the `values.yaml` file and avoid making changes directly to the generated deployment, service, or ingress templates.

For more information on Helm and its capabilities, refer to the official Helm documentation: [https://helm.sh/docs/](https://helm.sh/docs/)

If you have any questions or issues related to this repository or the deployment process, please feel free to open an issue or reach out to the repository owner.

## Azure Pipeline Variables
Before running the pipeline, make sure to define the following variables in your Azure Pipeline configuration:

ACR_ServiceConnection: The name or ID of the Azure Container Registry (ACR) service connection.
imageRepo: The name of the repository for the Docker image.
imageRegistry: The URL or address of the container registry.
These variables are used within the pipeline to configure the Docker image build and deployment.