# ICOS Scaphandre

## Description
Scaphandre is a <b>monitoring agent</b>, dedicated to capturing <b>energy consumption metrics</b>. Its primary purpose is to facilitate the measurement and understanding of energy consumption patterns in technological services, which is crucial for steering the tech industry towards greater sustainability.

Scaphandre exposes a wide array of metrics, which can be found [here](https://hubblo-org.github.io/scaphandre-documentation/references/metrics.html)

<b>For more detailed information about the Scaphandre project, please refer to:</b>:  
- The [GitHub repository of the Scaphandre project](https://github.com/hubblo-org/scaphandre/tree/main)
- The [documentation site of Scaphandre](https://docs.rs/scaphandre/latest/scaphandre/)
- The [simplified guide for Scaphandre installation](https://hubblo-org.github.io/scaphandre-documentation/index.html)
  
Within the ICOS suite, Scaphandre is utilized alongside the ICOS Telemetry. It is compatible with all CPU architectures except those in the ARM family. The metrics exposed from Scaphandre are scraped from Prometheus/Thanos of ICOS Telemetry Controller.
    
## Build, Package, Deploy and Test Scaphandre

### Build
The main repository for Scaphandre is located [here](https://github.com/hubblo-org/scaphandre/tree/main).  
From there, you can either create a custom image or use the [official Scaphandre image](https://hubblo-org.github.io/scaphandre-documentation/tutorials/kubernetes.html).

### Packaging
To begin the packaging process after cloning the repository, use the following command:
```shell 
helm package ./ -d ./releases
```
This command will create a new release, based on the values specified in the ```Chart.yaml``` file and the appropriate image set in the ```values.yaml``` file and place it in the ```releases``` folder.  

### Deploy
Deploy the created release with ```helm```  to the desired Kubernetes cluster using the following command:
```shell
helm install {{release-name}}
```
The Helm daemonset will deploy a Scaphandre instance to all available nodes of the cluster. During deployment, you may use the following parameters if necessary:
| Name |Description | Value |
|----------------------------|----------------------------------------------------------------------------------------|-------|
| `serviceMonitor.enabled` | Create ServiceMonitor Resource for scraping metrics using PrometheusOperator | false |
| `serviceMonitor.namespace` | The namespace in which the ServiceMonitor will be created (default is to use the same namespace as the Helm chart) | `default` |
| `serviceMonitor.interval`  | The interval at which metrics should be scraped | `30s`  |

### Testing
Scaphandre tests are automatically triggered during the deployment process:  
  
-  ```test-scaphandre-node-cpu-arch-pre-install```: This test is executed before Scaphandre installation on each node to ensure that the CPU architecture of each node is not ARM. The test will fail if all of the nodes have an ARM architecture.

- ```test-scaphandre-status-check```: This test runs after the deployment of each Scaphandre instance to verify that each pod is in a running state. The test will fail if all pods are not in a running state. You can re-run the test if necessary with the following command:
```shell 
helm test {{release-name}}
```

# Legal
The ICOS Scaphandre is released under the Apache 2.0 license.
Copyright © 2022-2024 National and Kapodistrian University of Athens. All rights reserved.

🇪🇺 This work has received funding from the European Union's HORIZON research and innovation programme under grant agreement No. 101070177.
