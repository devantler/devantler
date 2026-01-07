---
layout: page
title: Libraries
permalink: /libraries
image: "assets/images/work.jpg"
---

[![github-sponsor-button](https://img.shields.io/static/v1?label=Sponsor&message=%E2%9D%A4&logo=GitHub&color=%23fe8e86)](https://github.com/sponsors/devantler)

## Deprecated

The .NET libraries previously listed on this page have been **archived** and are no longer actively maintained.

These libraries were originally created to support [KSail](https://github.com/devantler-tech/ksail) when it was built in .NET. In December 2025, KSail was [rewritten in Go](/2025/05/07/building-ksail-from-shell-to-dotnet-to-go), which allowed the tool to embed Kubernetes tooling as native Go libraries rather than wrapping external binaries.

The archived libraries included:

- .NET Kubernetes Generator
- .NET Kubernetes Provisioner
- .NET Kubernetes Validator
- .NET Container Engine Provisioner
- .NET Secret Manager
- .NET Template Engine
- .NET Keys
- .NET CLI Runner
- Various .NET CLI wrappers (Age, Flux, Helm, K3d, K9s, Kind, Kubeconform, Kubectl, Kustomize, SOPS)

If you're looking for Kubernetes tooling, check out [KSail](https://github.com/devantler-tech/ksail) — it bundles all of this functionality into a single Go binary.
