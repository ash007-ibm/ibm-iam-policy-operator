# ibm-iam-policy-operator

An operator that deploys the IAM policy controller which detects the cluster administrator role and IAM role binding violations

> **Important**: Do not install this operator directly. Only install this operator using the IBM Common Services Operator. For more information about installing this operator and other Common Services operators, see Installer documentation. If you are using this operator as part of an IBM Cloud Pak, see the documentation for that IBM Cloud Pak to learn more about how to install and use the operator service. For more information about IBM Cloud Paks, see IBM Cloud Paks that use Common Services.

## Supported platforms

Red Hat OpenShift Container Platform 4.3 or newer installed on one of the following platforms.

- Linux x86_64
- Linux on Power (ppc64le)
- Linux on IBM Z and LinuxONE

## Operator versions

- 1.0.0

## Prerequisites

Before you install this operator, you need to first install the operator dependencies and prerequisites:

- For the list of operator dependencies, see the IBM Knowledge Center [Common Services dependencies documentation](http://ibm.biz/cpcs_opdependencies).

- For the list of prerequisites for installing the operator, see the IBM Knowledge Center [Preparing to install services documentation](http://ibm.biz/cpcs_opinstprereq).

- ibm-iam-policy-operator must run in the `ibm-common-services` namespace

## SecurityContextConstraints Requirements

The ibm-iam-policy-operator supports running with the OpenShift Container Platform 4.3 default restricted Security Context Constraints (SCCs).

For more information about the OpenShift Container Platform Security Context Constraints, see [Managing Security Context Constraints](https://docs.openshift.com/container-platform/4.3/authentication/managing-security-context-constraints.html).

OCP 4.3 restricted SCC:
```yaml
allowHostDirVolumePlugin: false
allowHostIPC: false
allowHostNetwork: false
allowHostPID: false
allowHostPorts: false
allowPrivilegeEscalation: true
allowPrivilegedContainer: false
allowedCapabilities: null
apiVersion: security.openshift.io/v1
defaultAddCapabilities: null
fsGroup:
  type: MustRunAs
groups:
- system:authenticated
kind: SecurityContextConstraints
metadata:
  annotations:
    kubernetes.io/description: restricted denies access to all host features and requires
      pods to be run with a UID, and SELinux context that are allocated to the namespace.  This
      is the most restrictive SCC and it is used by default for authenticated users.
  creationTimestamp: "2020-03-27T15:01:00Z"
  generation: 1
  name: restricted
  resourceVersion: "6365"
  selfLink: /apis/security.openshift.io/v1/securitycontextconstraints/restricted
  uid: 6a77775c-a6d8-4341-b04c-bd826a67f67e
priority: null
readOnlyRootFilesystem: false
requiredDropCapabilities:
- KILL
- MKNOD
- SETUID
- SETGID
runAsUser:
  type: MustRunAsRange
seLinuxContext:
  type: MustRunAs
supplementalGroups:
  type: RunAsAny
users: []
volumes:
- configMap
- downwardAPI
- emptyDir
- persistentVolumeClaim
- projected
- secret
```

## Documentation

To install the operator with the IBM Common Services Operator, follow the installation and configuration instructions within the IBM Knowledge Center.

- If you are using the operator as part of an IBM Cloud Pak, see the documentation for that IBM Cloud Pak. For a list of IBM Cloud Paks, see [IBM Cloud Paks that use Common Services](http://ibm.biz/cpcs_cloudpaks).

- If you are using the operator with an IBM Containerized Software, see the IBM Cloud Platform Common Services Knowledge Center [Installer documentation](http://ibm.biz/cpcs_opinstall).

- For more information, see the [IBM Cloud Platform Common Services documentation](http://ibm.biz/cpcsdocs).

## Developer guide

As a developer, if you want to build and test this operator to try out and learn more about the operator and its capabilities, you can use the following developer guide. The guide provides commands for a quick installation and initial validation for running the operator.

> **Important**: The following developer guide is provided as-is and only for trial and education purposes. IBM and IBM Support does not provide any support for the usage of the operator with this developer guide. For the official supported install and usage guide for the operator, see the the IBM Knowledge Center documentation for your IBM Cloud Pak or for IBM Cloud Platform Common Services.

### Overview

- To learn more about how the ibm-iam-policy-operator was implemented, see [Operator Guidelines](https://github.com/operator-framework/getting-started#getting-started) and [Operator SDK](https://sdk.operatorframework.io/docs/)

- All of the resources that were created by our Helm chart are now created by a controller.

- `ibm-iam-policy-operator` has two CRDs:
  1. `PolicyController` - Configurations for the `iam-policy-controller` deployment.
  1. `IamPolicy` - generated by `iam-policy-controller` repo.

## Configuration

- If a field in a spec is omitted, the default value will be used.
- The list of all `ibm-iam-policy-operator` settings can be found in the IBM Cloud Platform Common Services Knowledge Center [Configuration Documentation](http://ibm.biz/cpcs_opinstall).

### Developer Guide

- These steps are based on [Operator Framework: Getting Started](https://github.com/operator-framework/getting-started#getting-started) and [Creating an App Operator](https://github.com/operator-framework/operator-sdk#create-and-deploy-an-app-operator).

- Repositories: [ibm-iam-policy-operator](https://github.com/IBM/ibm-iam-policy-operator)
- Set the Go environment variables:

```bash
  export GOPATH=/home/<username>/go
  export GO111MODULE=on
  export GOPRIVATE="github.ibm.com"
```

- For more information, follow the Operator SDK [Quickstart Guide](https://sdk.operatorframework.io/docs/building-operators/golang/quickstart/)

- **IMPORTANT**: Anytime you modify `<kind>_types.go`, you must run `make generate` and `make generate-all` to update the CRD and the generated code.

### Testing

#### Prerequisites for building the operator locally

- [Install linters](https://github.com/IBM/go-repo-template/blob/master/docs/development.md)

#### Run the operator locally

- `make install-crd`
- `make run`
- Run tests on the cluster.
- `make uninstall`

#### Deploy the operator

- `make install`
- Run tests on the cluster.
- `make uninstall`

#### Installing by using the OCP Console

1. Create the `ibm-common-services` namespace.
1. Create the [CatalogSource](https://github.com/IBM/ibm-common-service-operator/blob/master/docs/install.md#1create-catalogsource) in your cluster.
1. Select the `Operators` tab and in the drop-down select `OperatorHub`.
1. Search for the `ibm-iam-policy-operator`.
1. Install the operator in the `ibm-common-services` namespace.
1. Run tests on cluster

#### Test Framework

- To run unit tests use, `make test`

#### Debugging the Operator

Run these commands to collect information about the iam-policy-operator and iam-policy-controller deployments.

1. `kubectl get pods -n ibm-common-services | grep iam-policy`
1. `kubectl get serviceaccount -n ibm-common-services | grep iam-policy`

These steps verify:

- The ibm-iam-policy-operator is running.
- The iam-policy-controller is running.

Run these commands to collect logs:

1. `kubectl logs <ibm-iam-policy-operator-pod>  -n ibm-common-services`
1. `kubectl logs <iam-policy-controller-pod> -n ibm-common-services`

#### End-to-End testing

- For more instructions on how to run end-to-end testing with the Operand Deployment Lifecycle Manager, see [ODLM guide](https://github.com/IBM/ibm-common-service-operator/blob/master/docs/install.md).
