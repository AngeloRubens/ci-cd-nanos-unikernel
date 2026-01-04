# 🚀 GitOps Nanos Unikernel Image Pipeline

![GitHub Actions](https://img.shields.io/github/actions/workflow/status/AngeloRubens/ci-cd-nanos-unikernel/.github/workflows/build.yml?label=CI&logo=github&style=flat-square)
![License](https://img.shields.io/github/license/AngeloRubens/ci-cd-nanos-unikernel?style=flat-square)
![GitHub stars](https://img.shields.io/github/stars/AngeloRubens/ci-cd-nanos-unikernel?style=flat-square)
![GitHub forks](https://img.shields.io/github/forks/AngeloRubens/ci-cd-nanos-unikernel?style=flat-square)
![GitHub issues](https://img.shields.io/github/issues/AngeloRubens/ci-cd-nanos-unikernel?style=flat-square)

![NanoVMs](https://img.shields.io/badge/NanoVMs-Nanos-000000?style=flat-square&logo=nanovms&logoColor=white)
![Unikernel](https://img.shields.io/badge/Unikernel-Yes-4c1?style=flat-square)
![Java](https://img.shields.io/badge/Java-21%2B-orange?style=flat-square&logo=openjdk&logoColor=white)
![GitOps](https://img.shields.io/badge/GitOps-Enabled-blue?style=flat-square)
![Hypervisor](https://img.shields.io/badge/Hypervisor-First-purple?style=flat-square)

Build **Java Application Server Unikernel images** using  
**GitHub Actions**, **NanoVMs (Nanos)** and **GitOps principles**.

> No containers.  
> No general-purpose OS.  
> No Kubernetes required.

---

## ✨ What is this?

This repository provides a **GitHub Actions CI pipeline** that builds **immutable Nanos Unikernel images** containing:

- ☕ a Java Application Server  
  *(Payara, GlassFish, TomEE, …)*
- 🧩 a selectable Java Runtime  
  *(Azul, Temurin, Semeru, …)*
- 🔐 a minimal and secure runtime environment

The result is a **single bootable image** that runs **directly on a hypervisor**.

![Deploy]([https://github.com/AngeloRubens/ci-cd-nanos-unikernel/blob/main/cloud_deploy_diagram.svg])

---

## 🧠 Why Unikernels?

Most Java workloads today rely on:

- containers
- full Linux distributions
- large orchestration stacks

This project demonstrates an **alternative deployment model**:

> **Build once → boot directly as a Unikernel.**

### Benefits

- ⚡ very fast boot times
- 🔒 minimal attack surface
- 📦 fully immutable runtime
- 🧠 simpler operational model
- ☁️ cloud & on-prem friendly

---

## 🧱 Architecture (GitOps)

![GitOps Nanos Pipeline](docs/gitops-nanos-swimlane.svg)

**Git is the source of truth**.  
**CI reconciles it into a bootable Unikernel image**.  
**Runtime becomes trivial**.

---

## 🛠️ What the pipeline does

The workflow is triggered via `workflow_dispatch` and performs:

### 1️⃣ Checkout repository

The repository defines the **desired state** and contains:

- `config.json` → Nanos configuration
- `workflow.yml` → CI definition
- documentation and diagrams

---

### 2️⃣ Prepare CI environment

The GitHub Actions runner installs:

- 🖥️ **QEMU** (for image build / local boot)
- ⚙️ **ops** (NanoVMs build tool)

No Docker daemon required.  
No privileged containers.

---

### 3️⃣ Download the Application Server

Based on user inputs, the pipeline:

- downloads an Application Server archive (`.zip` / `.tar.gz`)
- extracts it dynamically
- copies **only runtime files** into the Nanos filesystem

The build does **not depend on archive directory names**.

---

### 4️⃣ Build a minimal Nanos filesystem

Example layout:

```
nanos-root/
├── bin/
├── lib/
├── <application-server>/
├── etc/
│   └── hosts
```

Example `/etc/hosts`:

```
127.0.0.1 localhost
10.0.2.15 10-0-2-15
```

No shell.  
No package manager.  
No OS services.

---

### 5️⃣ Select the Java Runtime

The Java Runtime is selected via workflow inputs, e.g.:

```
AngeloRubens/AzulJREx64Linux:25.0.1
```

Any Java runtime compatible with `ops pkg` can be used.

---

### 6️⃣ Build the Unikernel image

Core build step:

```bash
ops image create \
  --imagename <image-name> \
  --package <java-runtime> \
  -c config.json
```

This produces:

- 🧱 a single immutable Nanos image
- ☕ Java + Application Server
- ☁️ ready for hypervisor boot

---

### 7️⃣ Publish the image artifact

The pipeline:

- discovers the generated image in `~/.ops/images/`
- fails if no image is produced
- uploads the image as a GitHub Actions artifact

The artifact can be:

- downloaded locally
- uploaded to a cloud provider
- used in downstream deployment pipelines

---

## 🔐 Security model

Security is provided by design, not by hardening:

- ❌ no shell access
- ❌ no SSH
- ❌ no package manager
- ❌ no mutable system state
- ✅ hardware isolation via hypervisor

The resulting image is **secure by construction**.

---

## 🔄 GitOps philosophy

This project applies GitOps principles to Unikernels:

- Git defines the desired state
- CI builds immutable artifacts
- environments are disposable
- rollback = boot a previous image

---

## ▶️ Usage

1. Fork this repository
2. Go to **Actions** → **Build Nanos Image**
3. Run the workflow manually
4. Choose:
   - Application Server
   - Java Runtime + version
5. Download the resulting Unikernel image

---

## 🧩 Supported & Planned

### Supported

- ✅ Payara 7
- ✅ Azul JRE
- ✅ GitHub Actions

### Planned

- 🔜 GlassFish
- 🔜 TomEE
- 🔜 Multiple Java runtimes
- 🔜 OCI / AWS / GCP image export
- 🔜 Automated deployment examples

---

## 🤝 Contributing

Contributions are welcome! 🙌

Ideas:

- add support for more application servers
- improve filesystem minimization
- add deployment examples
- improve documentation

Open an issue or submit a PR 🚀

---

## 📚 References

- 🧠 NanoVMs / Nanos — https://nanovms.com
- ⚙️ ops tool — https://ops.city
- 🔄 OpenGitOps — https://opengitops.dev

---

## 📜 License

Licensed under the Apache License 2.0.

> Built with ❤️ for people who want to run Java without containers.
