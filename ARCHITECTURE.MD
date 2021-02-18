# Architecture

This document describes the high-level architecture of Kube-OVN. 
If you want to understand the components CRDs and workflows, you are in the right place.

On the high-level, Kube-OVN acts as a bridge between Kubernetes and OVN. 
That means Kube-OVN will translate Kubernetes network concepts like CNI, Service, NetworkPolicy into OVN network. 
Also, Kube-OVN brings OVN advance SDN functions like L2/L3 network, VPC, Gateway, QoS back to Kubernetes network.
At the same time, Kube-OVN embeds monitor and diagnose tools to enhance operation productivity.

## Components 

The components in Kube-OVN can be divided into three categories:
* The upstream OVN components
* The core controller components
* Monitor, diagnose and extension tools

### OVN components

These components are from OVN/OVS community which is a system that support logical network abstraction in virtual machines and container environments.
We highly recommend you to read [ovn-architecture(7)](https://www.mankier.com/7/ovn-architecture) first to get an overview of what OVN is and its architecture. 
In Kube-OVN we treat OVN as the SDN implementation, and try to use its API to integrate it into Kubernetes.

All the OVN components in Kube-OVN are packed into docker image and can run as Kubernetes workloads.

#### ovn-central

It's a deployment that run OVN management components including `ovn-nb`,`ovn-sb` and `ovn-norhtd`.
`ovn-nb` stores the logical network and provides api for controllers to manage the logical network. 
It's the main components that Kube-OVN controller will interact with. 

`ovn-sb` stores the logical flows that are generated from logical network in `ovn-nb` and physical network in each node.

`ovn-northd` do the translation from `ovn-nb` to `ovn-sb`

Multiple `ovn-central` pods will run in raft-cluster mode that provides high availability.

### ovs-ovn

It's a daemonset that runs openvswitch, ovsdb and ovn-controller in every node.
They are agents of `ovn-central` that translate the logical network into the real network.

### Core Controllers

These are the core components in Kube-OVN that act as the bridge between Kubernetes and OVN and implement all the advanced network functions.

#### kube-ovn-controller

#### kube-ovn-cni

#### kube-ovn-daemon

### Other Operation and maintenance tools

#### kube-ovn-speaker

#### kube-ovn-pinger

#### kube-ovn-monitor

#### kubectl-ko

## CRDs

## Workflows
