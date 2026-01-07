---
layout: page
title: Projects
permalink: /projects
image: "assets/images/work.jpg"
---

[![github-sponsor-button](https://img.shields.io/static/v1?label=Sponsor&message=%E2%9D%A4&logo=GitHub&color=%23fe8e86)](https://github.com/sponsors/devantler)

# Active Projects

These are the projects I am currently working on and actively maintaining. I'm always looking for contributors and feedback—feel free to visit each project's GitHub page and follow the contribution guidelines.

## [🛥️ KSail](https://github.com/devantler-tech/ksail) ![Go](https://img.shields.io/badge/Go-00ADD8.svg?style=for-the-badge&logo=Go&logoColor=white) ![Docker](https://img.shields.io/badge/Docker-2496ED.svg?style=for-the-badge&logo=Docker&logoColor=white) ![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5.svg?style=for-the-badge&logo=Kubernetes&logoColor=white)

![KSail](../assets/images/ksail-cli-dark.png)

A CLI tool for provisioning GitOps-enabled Kubernetes clusters. It embeds common Kubernetes tools ([kubectl](https://kubernetes.io/docs/reference/kubectl/), [Helm](https://helm.sh/), [Kind](https://kind.sigs.k8s.io/), [K3d](https://k3d.io/), [Flux](https://fluxcd.io/), [ArgoCD](https://argo-cd.readthedocs.io/)) as Go libraries, requiring only [Docker](https://www.docker.com/) as an external dependency.

- **Local Development**: Spin up a cluster with GitOps enabled to instantly deploy your [Flux Kustomizations](https://fluxcd.io/flux/components/kustomize/kustomizations/)
- **CI Pipelines**: Test that your GitOps configurations successfully deploy applications
- **Multiple Distributions**: Support for [Kind](https://kind.sigs.k8s.io/), [K3d](https://k3d.io/), and [Talos](https://www.talos.dev/) clusters
- **GitOps Engines**: Support for [Flux](https://fluxcd.io/) and [ArgoCD](https://argo-cd.readthedocs.io/)
- **Secret Management**: Built-in [SOPS](https://github.com/getsops/sops) integration for encrypted secrets

📖 [Documentation](https://ksail.devantler.tech) • 📝 [Read about the journey from .NET to Go](/2025/05/07/building-ksail-from-shell-to-dotnet-to-go/)

## [☸️ Platform](https://github.com/devantler-tech/platform) ![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5.svg?style=for-the-badge&logo=Kubernetes&logoColor=white) ![Flux](https://img.shields.io/badge/Flux-5468FF.svg?style=for-the-badge&logo=Flux&logoColor=white) ![Talos Linux](https://img.shields.io/badge/Talos-FF7300.svg?style=for-the-badge&logo=Talos&logoColor=white)

![Platform](../assets/images/platform.webp)

A [Flux](https://fluxcd.io/) GitOps-based Kubernetes cluster running on a Mac Mini and Raspberry Pis in my home. It demonstrates a dev-friendly approach to working with Kubernetes using [Talos Linux](https://www.talos.dev/) as the operating system.

I use this cluster to learn and experiment with new technologies—striving to implement the latest CNCF projects to keep my skills sharp. I also run self-hosted services for entertainment, personal projects, and to own my own data.

**Key Technologies**: [Cilium](https://cilium.io/) (CNI), [Traefik](https://traefik.io/) (Ingress), [cert-manager](https://cert-manager.io/) (TLS), [SOPS](https://github.com/getsops/sops) (Secrets)

## [🔄 Reusable Workflows](https://github.com/devantler-tech/reusable-workflows) ![GitHub Actions](https://img.shields.io/badge/GitHub%20Actions-2088FF.svg?style=for-the-badge&logo=GitHub-Actions&logoColor=white)

A collection of [reusable GitHub Actions workflows](https://docs.github.com/en/actions/using-workflows/reusing-workflows) that encapsulate common CI/CD patterns. Used across all DevantlerTech projects to ensure consistency and reduce duplication.

- **CI Workflows**: [Go](https://go.dev/) testing/linting, [.NET](https://dotnet.microsoft.com/) testing, documentation linting, GitOps validation
- **CD Workflows**: Cluster bootstrap, GitOps deploy, [GitHub Pages](https://pages.github.com/) publish, application/library releases
- **Automation**: Auto-merge for trusted bots, [semantic-release](https://semantic-release.gitbook.io/), TODO scanning, [Kyverno](https://kyverno.io/) policy sync

## [⚡ Actions](https://github.com/devantler-tech/actions) ![GitHub Actions](https://img.shields.io/badge/GitHub%20Actions-2088FF.svg?style=for-the-badge&logo=GitHub-Actions&logoColor=white)

A collection of [composite GitHub Actions](https://docs.github.com/en/actions/creating-actions/creating-a-composite-action) that provide small, reusable components for CI/CD workflows.

- **Auto Merge**: Approve and auto-merge PRs from trusted bots/users
- **Cleanup GHCR**: Clean up old [GitHub Container Registry](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry) packages
- **Flux GitOps Deploy**: Push manifests to OCI and deploy using [Flux](https://fluxcd.io/)
- **Setup KSail**: Install [KSail](https://github.com/devantler-tech/ksail) CLI via [Homebrew](https://brew.sh/)
- **Install Cilium/Flux**: Install [Cilium](https://cilium.io/) and [Flux](https://fluxcd.io/) in Kubernetes clusters
- **TODOs**: Create GitHub issues from TODO comments in code

---

# Inactive Projects

These projects are no longer actively maintained, but I still think they are interesting, and I would love to pick them up again if I find the time or opportunity to do so.

---

# Completed Projects

Projects that have reached their end of life or I no longer actively maintain.

## [🚚 OCI Artifacts](https://github.com/devantler/oci-artifacts) ![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5.svg?style=for-the-badge&logo=Kubernetes&logoColor=white) ![Open Container Initiative](https://img.shields.io/badge/Open%20Containers%20Initiative-262261.svg?style=for-the-badge&logo=Open-Containers-Initiative&logoColor=white)

![OCI Artifacts](../assets/images/oci-artifacts.webp)

A spin-off from my Homelab project that simplifies GitOps deployments by distributing [Kustomize](https://kustomize.io/) and [Flux HelmRelease](https://fluxcd.io/flux/components/helm/helmreleases/) components via [OCI](https://opencontainers.org/) registries. This approach—inspired by shared libraries in traditional programming—allows deploying applications with a single line of code while maintaining the flexibility of regular [Kustomize](https://kustomize.io/) and [Helm](https://helm.sh/) charts.

The OCI Artifacts use [Flux Post Build Variables](https://fluxcd.io/flux/components/kustomize/kustomizations/#post-build-variable-substitution) to inject values into components, exposing configurable settings that users can provide when deploying.

## [⬡ Data Product](https://github.com/devantler/data-product) ![.NET](https://img.shields.io/badge/.NET-512BD4.svg?style=for-the-badge&logo=dotnet&logoColor=white)

![Data Product](../assets/images/data-product.webp)

My master thesis project, where I built a data product inspired by the Data Mesh architectural pattern, and the Data Product concept from the book "Data Mesh: Delivering Data-Driven Value at Scale" by Zhamak Dehghani. The project is built in .NET and uses a .NETs Source Generators to generate the data product based on its schema, and a YAML configuration file for the data product. As such, users were able to define their data product in a YAML file, and reference their schema to generate a data product with support for REST, GrapQL, Streaming, Observability, and more out-of-the-box.

The project was build as a containerized application, such that it can be deployed to various container orchestrators, and it was built according to many of the best practices I have learnt to appreciate from my interest in CNCF projects.

If you are interested in reading my thesis, you can find it [here](../assets/pdfs/thesis.pdf).

## [✍🏻 Pandoc Plus](https://github.com/devantler/pandoc-plus) ![Markdown](https://img.shields.io/badge/markdown-%23000000.svg?style=for-the-badge&logo=markdown&logoColor=white) ![LaTeX](https://img.shields.io/badge/latex-%23008080.svg?style=for-the-badge&logo=latex&logoColor=white)

![Pandoc Plus](../assets/images/pandoc-plus.webp)

A docker image that packages pandoc with LaTeX, PlantUML, and lua filters, to create LaTeX-styled scientific papers with Markdown. The image is built to include all the necessary batteries to create high-quality scientific papers, supporting the most common features of LaTeX, with a simple Markdown syntax. Pandoc Plus also supports beautiful transformations of all Markdown syntax, such that transpiles to good LaTeX practices, and results in a beautiful PDF. It being a docker image, also makes it trivial to use in CI pipelines, to ensure your scientific papers compile correctly, and that you can easily share your work with others. This essentially allows you to write your scientific papers in accordance with best practices in software development, saving you lots of time and effort.

Pandoc Plus was developed as a side project to my master thesis, which was also a compiled by Pandoc Plus. If you are interested in seeing the output of Pandoc Plus, and how the projects source code is structured, you can find my Master Thesis [here](../assets/pdfs/thesis.pdf).

## [🤖 Star Wars Site](https://github.com/devantler/star-wars-site) ![.NET](https://img.shields.io/badge/.NET-512BD4.svg?style=for-the-badge&logo=dotnet&logoColor=white) ![Umbraco](https://img.shields.io/badge/Umbraco-3544B1.svg?style=for-the-badge&logo=Umbraco&logoColor=white) ![Nomad](https://img.shields.io/badge/Nomad-00CA8E.svg?style=for-the-badge&logo=Nomad&logoColor=white)

A Star Wars site I built as part of a hirement process at Umbraco, where I was tasked with building a site that showcased my skills in .NET and Umbraco. The site is built as a Blazor WebAssembly application, that fetches data from Umbraco Heartcore, and displays it in a Star Wars themes site. The site is built to be responsive and to be as fast as possible, and it uses a lot of the latest and greatest technologies in .NET and Umbraco. To make the project a bit more interesting I decided to deploy the site to a Nomad cluster, as this was a technology I was interested in learning more about at the time. Nomad was a great container orchestrator, but I found it to be a bit too underappreciated in the industry, hence I now use Kubernetes for most of my projects, as it is more widely adopted.

The site is no longer maintained, as it was connected to an Umbraco Heartcore instance that was only available during the hirement process. But the source code is still available!

---

# School Projects

Projects completed during my Software Engineering studies at the [University of Southern Denmark](https://www.sdu.dk/en). These showcase my dedication to exploring new technologies and approaches to software engineering.

## 🌏 Exploration of State-of-the-Art Technology, Architectures and Tools to Create Future-Proof Data Spaces

### ⭐️ Graded 12/12 <span style="float:right">10th semester (MSc thesis)</span>

![Data Space as a Data Mesh](../assets/images/data-space-as-a-data-mesh.png)

An exploration of whether a data space can be implemented as a data mesh, focusing on challenges and potential benefits for collaboration among actors in the Danish energy sector. The study employs constructivism and the constructive research approach as its methodology, utilizing the grounded theory method to conduct fieldwork and create theories and hypotheses from the results. The research encompasses topics such as the climate, the Danish energy sector, data spaces, and data mesh, emphasizing a prototype of a data mesh’s central component, a data product. The prototype demonstrates that the data mesh approach can be successfully applied to data spaces, enabling better separation of domains within sectors and enabling discoverability, observability, and governance. However, it lacks the features and the maturity to provide a production-ready and complete solution. Overall, this thesis contributes to the ongoing discussion on future-proof data spaces and provides insights into implementing a data space as a data mesh to achieve that goal.

If you are interested in reading my thesis, you can find it [here](../assets/pdfs/thesis.pdf).

## 📊 Power Price Assistant

### ⭐️ Graded 12/12 <span style="float:right">9th semester</span>

![Power Price Assistant](../assets/images/power-price-assistant.png)

A web app that simulates a system that can advise on what electricity provider to choose based on the user's electricity consumption patterns, and priorities.

## 🏗️ Simulated Assembly Line

### ⭐️ Graded 12/12 <span style="float:right">8th semester</span>

<video class="lazy" width="640" height=" 360" controls>
  <source data-src="../assets/videos/simulated-assembly-line.mp4"  type="video/mp4">
</video>

A simulated assembly line consisting of a self-constructed crane, a rotating disk, and a web camera. It was programmed by the group's own Domain-Specific Language (DSL), which generated a client that could execute the program. The client utilized MQTT to communicate with the embedded system.

## 🌊 EcoBeach

### ⭐️ Graded 7/12 <span style="float:right">7th semester</span>

![EcoBeach](../assets/images/ecobeach.png)

A big data system that scraped satellite imagery of beach geo-locations from the Sentinel-2 satellite and processed them to determine how shorelines have changed over time. An Android app was also built to visualize the data.

You can read the report [here](../assets/pdfs/ecobeach.pdf)

## ⬣ HexUML

### ⭐️ Graded 12/12 <span style="float:right">6th semester (BSc thesis)</span>

![HexUML](../assets/images/hexuml.png)

A generic transpiler framework capable of translating one text source into another, e.g., from Java to C#. The framework was used to generate AnyLogic models for a web-based application (EcosystemMapGenerator), that I built for SDU during my hire as a student programmer at Maersk Mc-Kinney Moller Institute from February 2021 to November 2021.
