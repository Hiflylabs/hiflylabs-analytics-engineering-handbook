# Python models

## Python model execution on BigQuery
dbt uses the Dataproc service to execute python models as spark jobs on BigQuery data. Two submission methods for python models are `cluster` and `serverless`. The `cluster` method requires an existing and running spark cluster, `serverless` method creates a batch job based on the configuration.

The `serverless` method is easier to set up and operate and probably more cost effective than the `cluster` method but its startup time can be longer. The `cluster` method requires ceration of a cluster beforehand (with a configuration that enables connection to BigQuery) and you have to make sure that it is in running state when you run your python model. It will not stop automatically when the execution ends, which will increase costs. 


## dbt BigQuery setup

To configure your BigQuery connection profile for python model execution you can reference dbt's documentation [here](https://docs.getdbt.com/docs/core/connect-data-platform/bigquery-setup#running-python-models-on-dataproc).



## Serverless configuration
GCP's documentation on the Spark batch workload properties can be found [here](https://cloud.google.com/dataproc-serverless/docs/concepts/properties). You can set these in your `profiles.yml` under `dataproc_batch.runtime_config.properties`.

### Using python packages
When submitting a serverless python model, a Dataproc batch job is created that uses a default container image for runtime. This default image has a number of python packages installed (eg. `pySpark`, `pandas`, `NumPy` ...). Documentation is unclear on the full list of preinstalled packages, in case you want to use a package that is not available, you can create a custom container image that contains your dependencies. Google's guide on building custom container images can be found [here](https://cloud.google.com/dataproc-serverless/docs/guides/custom-containers).
The image needs to be stored in GCP's Artifact Registry, and you need to reference the image in your dbt profile, setting `dataproc_batch.runtime_config.container_image` to the image url.


## Cluster configuration
If you want to run your python models on a dedicated cluster, that cluster must be able to access BigQuery and the GCS storage bucket. [This](https://github.com/GoogleCloudDataproc/initialization-actions/tree/master/connectors#bigquery-connectors) document show how to include the GCS connector and how to run a BigQuery connector script as an initialization action when creating the cluster.

## Other useful resources 

[How to build dbt Python models in BigQuery?](https://www.datafold.com/blog/dbt-python#how-to-build-dbt-python-models-in-bigquery)

