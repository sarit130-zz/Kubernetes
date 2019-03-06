# aws-cluster-autoscaler:V1.2.4 and Designed for kubernetes verison 1.10

# This is the Usage of the chart

## TL;DR:

```console
$ helm install stable/aws-cluster-autoscaler -f values.yaml
```
Where `values.yaml` contains:

```
autoscalingGroups:
  - name: your-asg-name
    maxSize: 10
    minSize: 1
```

## Introduction

This chart bootstraps an aws-cluster-autoscaler deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Prerequisites
  - Kubernetes 1.10
  - use in kube-system namespace

## Installing the Chart

In order for the chart to configure the aws-cluster-autoscaler properly during the installation process, you must provide some minimal configuration which can't rely on defaults. This includes at least one element in the `autoscalingGroups` array and its three values: `name`, `minSize` and `maxSize`. These parameters cannot be passed to helm using the `--set` parameter at this time, so you must supply these using a `values.yaml` file such as:

```
autoscalingGroups:
  - name: your-asg-name
    maxSize: 10
    minSize: 1
```

To install the chart with the release name `my-release`:

```console
$ helm install stable/aws-cluster-autoscaler --name my-release -f values.yaml --namespace kube-system
```

# Verification command 
# Reference: IMAGE README


## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```console
$ helm delete my-release --purge
```

##Prerequisites
## IAM Permissions
The worker running the cluster autoscaler will need access to certain resources and actions:

$kops edit ig nodes --state s3://bucketname
##Change the autoscaling group min and max

$kops edit cluster --state s3://bucketname

## add

```
spec:
  additionalPolicies:
    node: |
      [
        {
          "Effect": "Allow",
          "Action": [
            "autoscaling:DescribeAutoScalingGroups",
            "autoscaling:DescribeAutoScalingInstances",
            "autoscaling:SetDesiredCapacity",
            "autoscaling:DescribeLaunchConfigurations",
            "autoscaling:DescribeTags",
            "autoscaling:TerminateInstanceInAutoScalingGroup",
            "ec2:DescribeLaunchTemplateVersions"
          ],
          "Resource": ["*"]
        }
      ]

```
