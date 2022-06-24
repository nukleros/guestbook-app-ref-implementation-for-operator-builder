# Guestbook Application Reference Implementation for Operator Builder

This repository uses two projects to construct an operator, based on the Kubernetes sample [guestbook application](https://kubernetes.io/docs/tutorials/stateless-application/guestbook/).
  * [YAML Overlay Tool](https://github.com/vmware-tanzu-labs/yaml-overlay-tool)
    Used for taking the source manifests found in the Kubernetes documentation and injecting operator-builder workload markers to prepare for building an operator.
  * [Operator Builder](https://github.com/nukleros/operator-builder)
    Used for constructing a fully functional operator from source Kubernetes manifests that have been marked up for CRD customization.


## How to use this repository

* Clone the repository

* Overlay the source manifests: `cd .yot; yot -i yot.yaml`  

* Initialize the repository for Operator Builder:

```bash
# assuming you're in .yot still
cd ..
go mod init guestbook-app

operator-builder init \
    --workload-config .source/workload.yaml \
    --skip-go-version-check
```

* Build the guestbook operator:

```bash
operator-builder create api \
    --workload-config .source/workload.yaml \
    --controller \
    --resource
```

* Install the guestbook CRD to your cluster:

```bash
make install
```

* Run the operator locally for testing purposes:

```bash
make run
```

* Since the controller is occupying your terminal, you'll want to start up another terminal to install the sample CR to your Kubernetes cluster:

```bash
kubectl apply -f config/samples
```

You should now see the guestbook application get deployed into your cluster.

Please reference the [operator-builder](https://github.com/nukleros/operator-builder) for all the details on building operators.
