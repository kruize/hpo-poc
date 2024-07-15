# HPO with raytune POC on OpenShift AI

This example demonstrates how to use raytune for hyperparameter optimisation using raytune on OpenShift AI.

> [!IMPORTANT]
> This example has been tested with the configurations listed in the [validation](#validation) section.
> Its configuration space is highly dimensional, with application configuration tighly coupled to runtime / hardware configuration.
> It is your responsibility to adapt it, and validate it works as expected, with your configuration(s), on your target environment(s).

## Requirements

* An OpenShift cluster (4.0+) with OpenShift AI (RHOAI) 2.10+ installed:
  * The `codeflare`, `dashboard`, `ray` and `workbenches` components enabled;
* Sufficient worker nodes for your configuration(s) with Nvidia GPUs (Ampere-based recommended);
* An AWS S3 bucket to store experimentation results.

## Setup

* Access the OpenShift AI dashboard, for example from the top navigation bar menu:
![](./docs/01.png)

* Log in, then go to Data Science Projects and create a project: 
![](./docs/02.png)

* Once the project is created, click on Create a workbench:
![](./docs/03.png)

* Add a cluster storage:
![](./docs/04.png)

* With the following details:
![](./docs/05.png)

* Then create a workbench with the following settings:
![](./docs/06.png)

* Add data connection with relevant details of your S3 storage bucket:
![](./docs/07.png)

### Setting Up Model Registry

To install the Model Registry Controller, follow these steps:

1. Clone and navigate to the Model Registry Operator repository:

    ```bash
    git clone https://github.com/opendatahub-io/model-registry-operator.git
    cd model-registry-operator
    ```
2. Use the provided Makefile to deploy the operator. Specify the image location using the `IMG` argument. For example, to deploy from a latest image hosted on Quay.io, run:

    ```bash
    make deploy IMG=quay.io/opendatahub/model-registry-operator
    ```

This command will deploy the Model Registry Controller using the specified image.

### Starting the Model Registry Service

After deploying the Model Registry Controller, you can start the Model Registry service with either PostgreSQL or MySQL as the backend database.
To start the Model Registry service with PostgreSQL, use the following command:

```bash
kubectl apply -k config/samples/postgres
```

### Running the codeflare examples

* Open the workbench:
![](./docs/08.png)

* Clone the following repository (https://github.com/kruize/hpo-poc.git):
![](./docs/09.png)
![](./docs/10.png)

* Navigate to the demos folder (hpo-poc/demos/)

* Repeat the following steps for each of the three files:
    * Raytune-oai-demo-mlmd.ipynb
    * Raytune-oai-demo-mlmd.ipynb
    * Raytune-oai-MR-gRPC-demo.ipynb

* In the openshift console open “copy login token”:
![](./docs/12.png)

* Replace the TOKEN and SERVER variables with those from the login command:
![](./docs/13.png)

* Restart and run the kernel, the results should be generated.
