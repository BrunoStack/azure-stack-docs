---
title: Troubleshoot the "Failed to either reach kube-apiserver or control plane IP of Kubernetes cluster from Arc Resource Bridge IP" error
description: Learn how to troubleshoot the "failed to either reach kube-apiserver or control plane IP of Kubernetes cluster from Arc Resource Bridge IP error" when you try to create and deploy an AKS enabled by Arc cluster.
ms.topic: troubleshooting
author: sethmanheim
ms.author: sethm
ms.date: 05/29/2024
ms.reviewer: abha

#Customer intent: As an Azure Kubernetes user, I want to troubleshoot the "failed to either reach kube-apiserver or control plane IP of Kubernetes cluster from Arc Resource Bridge IP error" error code so that I can successfully start or create and deploy an Azure Kubernetes Service Arc cluster.

---

# Troubleshoot the failed to either reach kube-apiserver or control plane IP of Kubernetes cluster from Arc Resource Bridge IP error

This article describes how to identify and resolve the "failed to either reach kube-apiserver or control plane IP of Kubernetes cluster from Arc Resource Bridge IP" error that occurs when you try to create and deploy a Microsoft Azure Kubernetes Service Arc cluster.

## Symptoms

When you try to create an AKS cluster, you receive the following error message:

```output
Failed to either reach kube-apiserver or control plane IP %s:%d of Kubernetes cluster %s from Arc Resource Bridge IP %v. Read aka.ms/aks-arc-kubeapiserver-unreachable for more information.
```

## Possible causes and follow-ups

- Check whether the logical network into which you deployed the AKS cluster is reachable from the management network. The management network is a [block of 6 IP addresses that were allocated during Azure stack HCI deployment](/azure-stack/hci/deploy/deploy-via-portal#specify-network-settings).
- Check whether the `control plane IP` address of the AKS cluster is reachable from the management network. You can find the control plane IP address by running the [`az aksarc show`](/cli/azure/aksarc#az-aksarc-show) command.
- One possible reason for connectivity issues here is if the Arc Resource Bridge is in a different vlan than the logical network in which you created the AKS cluster. If the Arc Resource Bridge is in a different vlan, ensure that cross-vlan communication is enabled.
- Another frequent issue occurs if you have a firewall that isolates the management network from logical networks. Ensure that you opened the required ports so the management network can reach AKS Arc VMs. Review [the network requirements](aks-hci-network-system-requirements.md#network-port-and-cross-vlan-requirements) for more information about required ports.
- Duplicate IPs and IP address collisions are other reasons why the API server might be unreachable. Ensure that you didn't use the IP addresses provided in the management network anywhere else. Likewise, ensure that the control plane IP provided during the AKS cluster creation operation isn't used anywhere else.

## Contact Microsoft Support

If the problem persists, collect the following information before [creating a support request](aks-troubleshoot.md#open-a-support-request). Collect [AKS cluster logs](get-on-demand-logs.md) before creating the support request.

## Next steps

- [Review known issues for AKS on Azure Stack HCI 23H2](aks-known-issues.md)
- [Review AKS on Azure Stack HCI 23H2 architecture](cluster-architecture.md)
- [Review networking prerequisities for AKS on Azure Stack HCI 23H2](aks-hci-network-system-requirements.md)
