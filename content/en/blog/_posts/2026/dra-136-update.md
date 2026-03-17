---
layout: blog
title: "Kubernetes v1.36: DRA has graduated to GA"
slug: dra-136-updates
draft: true
date: XXXX-XX-XX
author: >
  The DRA team
---

Dynamic Resource Allocation (DRA) has fundamentally changed how we handle hardware
accelerators and specialized resources in Kubernetes. In the v1.36 release, DRA
continues to mature, bringing a wave of feature graduations, critical usability
improvements, and new capabilities that bridge the gap between workload deployment
and specialized hardware orchestration.

Whether you are managing massive fleets of GPUs, partitioning devices, or simply looking
for better ways to define resource fallback options, the v1.36 updates to DRA have
something for you. Let's dive into the new features and graduations!

## Feature Graduations

The community has been hard at work stabilizing core DRA concepts. In Kubernetes 1.36,
several highly anticipated features have graduated to Beta and Stable.

**Prioritized List (Stable)**

Hardware heterogeneity is a reality in most clusters. With the Prioritized List feature
now graduating to Stable, you can confidently define fallback preferences when requesting
devices. Instead of hardcoding a request for a specific device model, you can specify an
ordered list of preferences (e.g., "Give me an H100, but if none are available, fall back
to an A100"). The scheduler will evaluate these requests in order, drastically improving
scheduling flexibility and cluster utilization.

**Partitionable Devices (Beta)**

Hardware accelerators are powerful, and sometimes a single workload doesn't need an
entire device. The Partitionable Devices feature, now in Beta, provides native DRA
support for carving physical hardware into smaller, logical instances (such as
Multi-Instance GPUs or MIGs). This allows administrators to safely and efficiently
share expensive accelerators across multiple Pods.

**Device Taints (Beta)**

Similar to how you can taint a Kubernetes Node, you can now apply taints directly to
specific DRA devices. Graduating to Beta in this release, Device Taints allow cluster
administrators to reserve specific hardware for dedicated teams, specialized workloads,
or experimental environments. Only Pods with the corresponding tolerations will be
permitted to claim and bind to these tainted devices.

**Device Binding Conditions (Beta)**

Observability during the Pod scheduling phase has historically been tricky. With
Device Binding Conditions moving to Beta, Kubernetes now exposes detailed, structured
condition statuses natively on Pods and ResourceClaims. This makes it significantly
easier to debug why a Pod is stuck pending, whether it's waiting on device allocation,
binding, or a specific DRA driver response.

**Extended Resource Support (Beta)**

As DRA becomes the standard for resource allocation, bridging the gap with legacy
systems is crucial. The ability to request traditional node-level Extended Resources
through the DRA API has graduated to Beta. This provides a unified, consistent API
surface for users, allowing them to use DRA's advanced semantics even for resources
exposed via older device plugins.

## New Features

Beyond stabilizing existing capabilities, v1.36 introduces foundational new features
that expand what DRA can do.

**ResourceClaim Support for Workloads**

Historically, integrating DRA ResourceClaims with higher-level workload controllers
(like Deployments, StatefulSets, and Jobs) required complex workarounds or third-party
operators. In v1.36, Kubernetes introduces native workload support for ResourceClaims.
Similar to how a StatefulSet dynamically provisions PersistentVolumeClaims using
VolumeClaimTemplates, workloads can now seamlessly generate and manage the lifecycle
of ResourceClaims natively. This is a massive leap forward for the developer experience.

**DRA for Native Resources**

Why should DRA only be for external accelerators? In v1.36, we are introducing the first
iterations of using the DRA API to manage Kubernetes "Native" resources (like CPU and
Memory). By bringing CPU and memory allocation under the DRA umbrella, users can leverage
DRA's advanced placement, NUMA-awareness, and prioritization semantics for standard
compute resources, paving the way for incredibly fine-grained performance tuning.

**DRA Resource Availability Visibility**

One of the most requested features from cluster administrators has been better visibility
into hardware capacity. The new Resource Availability Visibility feature introduces
robust mechanisms to query and expose the total capacity, allocated usage, and available
pool of DRA resources across the cluster. This unlocks better integration with autoscalers,
dashboards, and capacity planning tools.

## Under the Hood: Controller Improvements

Improvements aren't just limited to user-facing APIs; the core controllers have also
received significant upgrades.

**Index-Based Naming and Lexicographical Sorting**

In v1.36, the ResourceSlice controller has been updated to use index-based naming, and
it now sorts resource slices and pools lexicographically. While this sounds like an
internal implementation detail, it has a profound impact on the system's predictability.
By enforcing a deterministic, sorted order for how resource slices are named and
processed, the controller prevents unnecessary churn, reduces edge-case race conditions
during rapid scaling events, and makes controller logs much easier to trace.

## Getting Involved

The rapid evolution of DRA is driven by the feedback and contributions of the community.
If you are interested in shaping the future of hardware management in Kubernetes, we
encourage you to join the conversations in SIG Node and SIG Scheduling.

Check out the official Kubernetes 1.36 release notes for a complete list of changes,
and happy scheduling!