---
title: Prometheus
description: How to setup Prometheus monitoring.
weight: 10
icon: setup
keywords: setup
---

In order to evaluate the quality gates, we have to set up monitoring to provide the needed data. The setup provided in this documenation is specific to the carts service that is used throughout all use cases. This requires that the created project is called *sockshop* and the service is called *carts* as described in the use case [onboarding a service](../onboard-carts-service).

## Setup Prometheus

1. Checkout the needed files.

    - If you have not yet onboarded the carts service, please execute the following commands to receive the needed files:
    
      ```console
      git clone --branch 0.5.0.beta https://github.com/keptn/examples.git --single-branch
      ```

      ```console 
      cd ./examples/onboarding-carts
      ```

    - If you already have [onboarded the carts service](../../usecases/onboard-carts-service/), navigate to the directory `examples/onboarding-carts`.

1. If Prometheus is currently not installed in your Kubernetes cluster, the following command will set up Prometheus and the Alerting Manager:

    ```console
    keptn configure monitoring prometheus --project=sockshop --service=carts
    ```
    **Note:** If the keptn CLI does not show any output for approximately 30 seconds, abort the command (i.e. press Ctrl + C). Afterwards, please retry this command.

## Verify installation

- To verify the Prometheus installation, retrieve the pods running in the `monitoring` namespace.

  ```console
  kubectl get pods -n monitoring
  ```

  ```console
  NAME                                     READY   STATUS    RESTARTS   AGE
  alertmanager-79f667b965-dm5nb            1/1     Running   0          21s
  prometheus-deployment-7d75b5fbdd-dpftz   1/1     Running   0          23s
  ```

Besides, you can browse to the Prometheus web interface:

1. Enable port-forwarding for the `prometheus-service`:

    ```console
    kubectl port-forward svc/prometheus-service 8080 -n monitoring
    ```

1. Open the URL [localhost:8080/targets](http://localhost:8080/targets) in your browser to see the three targets for the carts service:

    {{< popup_image link="./assets/prometheus-targets.png" caption="Prometheus Targets">}}

    **Note:** If you have not onboarded the carts service yet, the Prometheus dashboard will report _down_ in the state column.

## Uninstall Prometheus

If you want to uninstall Prometheus, there are scripts provided to do so. Uninstalling Keptn will not automatically uninstall Prometheus.

1. (optional) If you do not have the *examples* repository, clone the latest release using:

    ```console
    git clone --branch 0.5.0.beta https://github.com/keptn/examples.git --single-branch
    ```

1. Go to correct folder and execute the `uninstallPrometheus.sh` script:

    ```console
    cd examples/monitoring/prometheus
    ```

    ```
    ./uninstallPrometheus.sh
    ```