# A Geospatial Join Optimization for Federated GeoSPARQL querying

In the following we provide instuctions how to run the experiments shown in the paper.

## Prerequisites

The experiments are executed using the KOBE benchmarking engine.
For instructions on how to install KOBE in your system please visit the
[GitHub page](https://github.com/semagrow/kobe/) or the [KOBE User Guide](https://semagrow.github.io/kobe/). 

## How to run a query

In the following, we show the steps for deploying an experiment on geofedbench benchmark that comprises queries over a Semagrow federation of Strabon endpoints.
For example to run the first subset of the data validation query workload follow the commands below.

First, apply the template for Strabon and Semagrow:
```bash
kubectl apply -f strabontemplate.yaml
kubectl apply -f experiments/semagrowtemplate-gdo-dopt.yaml
kubectl apply -f experiments/semagrowtemplate-gdo-orig.yaml
```

Then, apply the benchmark:
```bash
kubectl apply -f benchmarks/gdobench-validation-queryset/gdobench-0.yaml
```

Before running the experiment, you should verify that the datasets are loaded. 
Use the following command:
```bash
kubectl -n gdobench get pods
```

When the datasets are loaded, you should get the following output:
```bash
NAME                STATUS
lucas-all-pod       Running
invekos-all-pod     Running
```

Check if the data are actually stored, for each pod.
For lucas-all-pod, for example, use the following command:
```bash
kubectl -n gdobench logs lucas-all-pod -c maincontainer | grep "Data Stored Successfully"
```
"Data Stored Successfully" should be printed in the output.

Proceed now with the execution of the experiment for the optimized Semagrow:
```bash
kubectl apply -f experiments/exp-gdo-dopt.yaml
```
and after it ends, you can try the other one (for the unoptimized Semagrow:

As previously, you can review the state of the experiment with the following command:
```bash
kubectl -n gdobench get pods
```

Before you run another query, delete all specifications as follows:
```bash
kubectl delete experiments.kobe.semagrow.org --all
kubectl delete benchmarks.kobe.semagrow.org --all
kubectl delete federatortemplates.kobe.semagrow.org --all
kubectl delete datasettemplates.kobe.semagrow.org --all
```
and then redo the steps from the beginning for the other queries of the benchmark.
 
## Source code

The source code of the version of Semagrow that contains the optimization can be found at
[./semagrow.tar.gz](./semagrow.tar.gz)
