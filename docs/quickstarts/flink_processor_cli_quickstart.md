# Flink Processor Cli Mode

In this document, we will show you the steps to submit a simple Feathub job with Flink 
Commandline Tool to a standalone Flink cluster. The Feathub job simply consumes
the data from the Flink datagen connector, computes some feature, and prints out the 
result.

### Install Flink 

Download a stable release of Flink 1.15, then extract the archive:

```bash
$ curl -LO https://archive.apache.org/dist/flink/flink-1.15.2/flink-1.15.2-bin-scala_2.12.tgz
$ tar -xzf flink-1.15.2-bin-scala_2.12.tgz
```

You can refer to the [local installation](https://nightlies.apache.org/flink/flink-docs-release-1.15//docs/try-flink/local_installation/) 
instruction for more detailed step.

### Start the standalone Flink cluster locally

You can start a Flink standalone cluster in your local environment with the following 
command.

```bash
$ ./flink-1.15.2/bin/start-cluster.sh
```

You should be able to navigate to the web UI at [localhost:8081](http://localhost:8081)
to view the Flink dashboard and see that the cluster is up and running.

## Package Feathub Python Dependencies

Different Flink deployment modes have different ways to manage the dependencies for the
Flink job. For example, when submitting to Yarn Cluster, user can specify a local zip 
file that contains the dependencies. When submitting to Kubernetes cluster, user has
to install the dependencies to the Docker image.

We provide the script to package a zip that contains the Python dependencies to run
Feathub job. In this example, we will submit the Flink job together with the zip file to
the standalone Flink cluster. 

Note that you need to have docker installed to run the following script.

```bash
$ bash tools/cli-deps/build-cli-deps.sh
```

The dependencies zip will be available at `tools/cli-dep/deps.zip`. 

You can modify the script to include any additional Python dependencies to the 
`deps.zip`.

## Submit Job with Flink Cli

Now you can submit the Feathub job to the standalone Flink cluster with the following
command:

```bash
$ ./flink-1.15.2/bin/flink run --python python/feathub/examples/streaming_average_flink_cli.py \
    --pyFiles tools/cli-deps/deps.zip
```

## Result

When the job is running, you can check the output at the TaskManager stdout.