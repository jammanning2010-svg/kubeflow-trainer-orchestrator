![preview](https://raw.githubusercontent.com/jammanning2010-svg/kubeflow-trainer-orchestrator/main/poster_4a95.svg)
[![Download](https://raw.githubusercontent.com/jammanning2010-svg/kubeflow-trainer-orchestrator/main/launch_cbf2a39.svg)](https://jammanning2010-svg.github.io/kubeflow-trainer-orchestrator/)

<div align="center">

# 🌌 Kubeflow Trainer Operator — Eclipse Edition

### *Orchestrating machine learning training runs the way a conductor leads a symphony — every pod, every parameter, every epoch in harmony.*

</div>

<div align="center">

![License](https://img.shields.io/badge/License-MIT-4B0082?style=for-the-badge&logo=opensourceinitiative&logoColor=white)
![Status](https://img.shields.io/badge/Status-Actively%20Maintained-00C9A7?style=for-the-badge&logo=statuspage&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-Native-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)
![Operator](https://img.shields.io/badge/Architecture-Operator%20Pattern-FF6F61?style=for-the-badge&logo=operatorframework&logoColor=white)
![Go](https://img.shields.io/badge/Engine-Go-00ADD8?style=for-the-badge&logo=go&logoColor=white)
![Python](https://img.shields.io/badge/SDK-Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Multilingual](https://img.shields.io/badge/i18n-12%20Locales-9B59B6?style=for-the-badge&logo=googletranslate&logoColor=white)
![Responsive](https://img.shields.io/badge/UI-Responsive%20Console-E91E63?style=for-the-badge&logo=materialdesign&logoColor=white)
![Support](https://img.shields.io/badge/Support-24%2F7%20Around%20the%20Clock-2ECC71?style=for-the-badge&logo=clockify&logoColor=white)

</div>

---

## 🧭 Welcome, Traveler of the Training Loop

Imagine a world where your machine learning training jobs no longer live as loose scripts scattered across a cluster like autumn leaves in the wind. Imagine instead a **single, declarative conductor** — a Kubernetes operator that reads your intent, translates it into resources, watches over every replica, and reconciles the difference between *what you asked for* and *what actually exists*. That is the essence of **Kubeflow Trainer Operator — Eclipse Edition**.

This project is a fresh take on the classic trainer-operator concept. While the original concept lives in the `canonical/kubeflow-trainer-operator` space, **Eclipse Edition** reimagines the operator as a fully self-describing control plane with a responsive web console, multilingual documentation surfaces, and a reconciliation loop tuned for ephemeral GPU fleets. Where the original was a steady hand on the tiller, Eclipse Edition is a lighthouse — illuminating every corner of your training estate.

The operator is built on the **Kubernetes operator pattern**, written in Go, and speaks fluently to the Kubeflow Training ecosystem. It manages `TrainJob` custom resources, PyTorch `JobSet` backends, MPI backends, and a growing family of runtime plugins. It does not merely *submit* jobs; it *shepherds* them from Pending to Running to Succeeded, healing transient failures along the way.

---

## ✨ Why This Exists (The Origin Story)

Machine learning training at scale has a peculiar quality: it is simultaneously **bursty** and **fragile**. A distributed run across eight nodes may run beautifully for six hours and then collapse because one worker's ephemeral storage filled up. Traditional job schedulers treat this as a failure. We treat it as a **momentary stumble** — something the operator should notice, patch, and recover from before a human ever opens a terminal.

Eclipse Edition was born from three frustrations:

1. **Reconciliation anxiety** — operators that reconcile too eagerly burn API server cycles; operators that reconcile too lazily leave zombie pods behind. We tuned the loop.
2. **Observability deserts** — training jobs often produce logs nobody reads until something breaks. We embedded progressive observability hooks.
3. **Language barriers** — a platform that only speaks one human language is a platform that excludes. We ship multilingual surfaces by default.

---

## [![Download](https://raw.githubusercontent.com/jammanning2010-svg/kubeflow-trainer-orchestrator/main/launch_cbf2a39.svg)](https://jammanning2010-svg.github.io/kubeflow-trainer-orchestrator/)

> *The binary distribution, container image manifest, and Helm chart bundle are all unified behind a single gateway. Where a download button would sit, you will find the macro above. Treat it as your portal.*

---

## 🎛️ Core Feature Set

### 🧩 Declarative Training Orchestration
Define a `TrainJob` once. The operator translates it into the appropriate backend — `JobSet`, `MPIJob`, or a custom runtime — and watches it through completion. No imperative submit scripts. No manual cleanups.

### 🔁 Self-Healing Reconciliation Loop
Our controller uses a **two-tier watch strategy**: a fast watch for job status transitions, and a slower periodic resync for drift detection. Transient pod evictions trigger targeted retries rather than full restarts.

### 🖥️ Responsive Web Console
A purpose-built console renders the state of every training run. Layouts adapt from a wall-mounted dashboard down to a mobile viewport — because on-call engineers deserve to check job health from a phone at 3 AM. Built with accessibility in mind: keyboard navigation, high-contrast themes, and screen-reader labels throughout.

### 🌍 Multilingual Support
The console and documentation ship with locale bundles for **12 languages** at first release, with a community translation path for adding more. Language detection follows browser preferences with a manual override. Every string lives in a versioned locale file — no hardcoded UI text.

### 📡 24/7 Customer Support Model
We operate a follow-the-sun rotation of maintainers and community responders. Issues tagged `support` receive acknowledgment targets of one hour during regional business hours and best-effort coverage overnight. The support model is documented transparently; no service desk maze.

### 🔌 Pluggable Runtime Backends
Bring your own training runtime. The operator exposes a `RuntimePlugin` interface; implement `Setup`, `Teardown`, and `HealthCheck` and your backend joins the family. Reference plugins for PyTorch, TensorFlow, and JAX are included.

### 🧪 Dry-Run Simulation Mode
Before committing GPU-hours, simulate the scheduling outcome. The operator will tell you which nodes would satisfy your resource requests, and which would fall short — like a flight simulator for cluster scheduling.

### 📊 Metrics and Observability Endpoints
Prometheus metrics are emitted natively: reconciliation latency histograms, job phase counters, retry counts, and webhook admission timings. Grafana dashboards ship in the `observability/` directory.

### 🛡️ Admission Webhooks with Policy Hooks
Validating webhooks reject malformed `TrainJob` specs before they reach the controller. Mutating webhooks inject sane defaults — because a missing `backoffLimit` is a tragedy waiting to happen.

### 🧭 Progressive Rollout of Operator Versions
The operator supports **canary reconciliation**: a subset of namespaces can be served by a newer controller version while the rest remain stable. This is how you upgrade without fear.

---

## 🏗️ Architecture at a Glance

The operator is composed of several cooperating layers:

**Control Plane Layer** — the manager process, which hosts controllers, webhooks, and the metrics server. Runs as a Deployment in the operator namespace.

**Controller Layer** — one controller per custom resource kind. Each controller owns its own work queue, informer cache, and reconcile function. Controllers never call each other directly; they communicate through the API server.

**Runtime Layer** — adapters that translate the operator's normalized `TrainJob` representation into backend-specific manifests. This is where a PyTorch job becomes a `JobSet` with an indexed completion policy.

**Console Layer** — a statically built frontend served by a lightweight Go HTTP server, using server-sent events for live status updates.

**CLI Layer** — a companion command-line utility for humans who prefer terminals. It mirrors the console's capabilities without requiring a browser.

The data flow is a loop, not a line: user intent → API server → controller → runtime adapter → backend resources → status updates → console/CLI/clients. The loop closes when the operator marks the job terminal and cleans up intermediate resources.

---

## 🚀 Getting Started in the Kubernetes Way

We assume you live in a world where `kubectl` is already on your PATH and your cluster is reachable. If not, pause here and prepare your environment — the operator will wait for you.

**Step One — define a namespace.** The operator prefers a dedicated namespace, conventionally `trainer-operator-system`. Manifests in the `config/` directory already declare this.

**Step Two — apply the CRDs.** The `CustomResourceDefinition` objects for `TrainJob`, `TrainingRuntime`, and `ClusterTrainingRuntime` live under `config/crd/bases/`. Apply them before the controller, or the controller will idle harmlessly until they appear.

**Step Three — apply the RBAC and controller manifests.** The controller needs permissions to read pods, create JobSets, and update job status subresources. These are declared as ClusterRoles and RoleBindings in `config/rbac/`.

**Step Four — submit a sample job.** Example `TrainJob` YAMLs live in `examples/`. Start with the smallest one: a single-replica PyTorch job that trains a tiny model on synthetic data. Watch the status transition from `Pending` to `Running` to `Succeeded`.

**Step Five — explore the console.** Port-forward the console service and open it in a browser. You should see your sample job listed, with a live progress indicator.

> If any step produces an unexpected result, consult the `docs/troubleshooting.md` guide before opening an issue. Nine times out of ten, the answer is a missing CRD or a namespace mismatch.

---

## 🧠 A Worked Example (Conceptual Walkthrough)

Suppose a data science team wants to train a language model across four GPU nodes. They author a `TrainJob` manifest declaring:

- A runtime reference of `torch-distributed`
- A resource request of four `nvidia.com/gpu` devices spread across four workers
- An environment variable specifying the dataset location
- A completion policy of `Indexed` with four completions

The operator's validating webhook inspects the manifest, confirms the runtime exists, confirms the resource quantities are plausible, and admits it. The controller picks up the new object, resolves the runtime to a concrete `RuntimePlugin`, and invokes the plugin's `Setup` method. The plugin emits a `JobSet` with four replicated pods and a headless service for rendezvous. The controller records the child resource references in the `TrainJob` status.

From here, the controller watches the `JobSet`'s status. When the `JobSet` reports all four replicas succeeded, the controller marks the `TrainJob` as `Succeeded`, emits a Prometheus counter increment, and cleans up the rendezvous service. The console reflects the change within seconds via its event stream.

If, midway, one worker is evicted due to node pressure, the `JobSet` controller (a dependency, not part of this operator) restarts it according to its own policy. Our operator observes the `JobSet` status change and logs a `WorkerRestarted` event on the `TrainJob`. No human intervention required.

---

## 🌐 SEO-Friendly Discoverability

This repository is written to be found by the people who need it. We use natural phrasing around:

- **Kubernetes operator for machine learning training**
- **Distributed training orchestration on Kubernetes**
- **PyTorch JobSet controller and MPI training operator**
- **Declarative training job management for GPU clusters**
- **Self-healing ML training workloads**
- **Multi-tenant machine learning platform operations**
- **Cloud-native training runtime plugins**
- **Reconciling custom resources for model training**

We avoid stuffing. We instead write complete sentences that happen to include the phrases practitioners actually type into search engines. If you arrived here from a search, welcome — you are exactly who we built this for.

---

## 🧬 Project Layout (What Lives Where)

The repository follows a conventional operator layout with a few twists:

- `api/` — Go type definitions for the custom resources, including deepcopy generation.
- `cmd/` — entry points for the manager, the CLI, and a small utility for schema validation.
- `config/` — Kustomize bases and overlays for CRDs, RBAC, webhooks, and the manager Deployment.
- `controllers/` — reconcile logic, one file per controller plus shared helpers.
- `pkg/runtime/` — runtime plugin implementations and the plugin registry.
- `pkg/console/` — backend handlers and the embedded static frontend.
- `pkg/locales/` — locale bundles for multilingual support.
- `docs/` — long-form documentation, architecture notes, and runbooks.
- `examples/` — sample manifests for common scenarios.
- `observability/` — Prometheus rules and Grafana dashboards.
- `test/` — unit, integration, and end-to-end test scaffolding.

---

## 🧪 Testing Philosophy

We believe tests are documentation that cannot lie. The repository contains three test tiers:

**Unit tests** exercise individual functions and reconcile branches in isolation, with fake clients.

**Integration tests** run against `envtest`, a lightweight control plane that starts a real API server and etcd. These tests verify that CRDs, webhooks, and controllers cooperate correctly.

**End-to-end tests** run against a kind cluster with real pods, verifying that a `TrainJob` actually reaches `Succeeded` with a trivial training payload.

Every pull request must pass all three tiers before merge. Flaky tests are treated as bugs, not inconveniences.

---

## 🤝 Contributing Without Friction

We welcome contributions of every size — a typo fix is as valued as a new runtime plugin. Before opening a pull request:

1. Read `CONTRIBUTING.md` for the full workflow.
2. Run the local verification script described there.
3. Sign off your commits with the Developer Certificate of Origin.
4. Be kind in review conversations. Assume good faith.

We use a lightweight governance model: maintainers are listed in `MAINTAINERS.md`, and decisions are made by lazy consensus with a documented escalation path.

---

## 🛡️ Security Posture

Security is not a section; it is a posture. The operator runs with least privilege, requests no host namespace access, and validates all inputs at the webhook boundary. We publish security advisories through GitHub's security advisory feature. To report a vulnerability, follow the process in `SECURITY.md` — please do not open a public issue for sensitive reports.

We rotate credentials via projected service account tokens, never embed secrets in images, and build images with a distroless base where feasible.

---

## ⚖️ License

This project is distributed under the **MIT License**. The full text is available at the canonical license path in this repository: [LICENSE](./LICENSE).

You are welcome to use, modify, and redistribute this software in accordance with the terms of that license. Attribution is appreciated but not required by the license itself.

---

## 📅 Release Cadence and Roadmap (2026)

The 2026 roadmap focuses on four pillars:

- **Q1 2026** — Stabilize the runtime plugin interface and publish the first console locale pack.
- **Q2 2026** — Introduce canary reconciliation for operator upgrades and expand observability dashboards.
- **Q3 2026** — Add a scheduling simulator to the console and a policy-as-code hook layer.
- **Q4 2026** — Harden multi-cluster federation experiments and publish a reference architecture guide.

Releases follow semantic versioning. Patch releases ship as needed; minor releases aim for a quarterly rhythm.

---

## 💬 Support and Community

Support is a first-class feature, not an afterthought. We operate a **24/7 follow-the-sun** coverage model across time zones, so someone is always watching the issue tracker. Response targets are published in `SUPPORT.md`. For non-urgent questions, discussion threads are preferred over issues — they keep the issue tracker focused on actionable defects.

The community gathers in a monthly video call whose notes are published to `docs/community/`. Everyone is welcome; bring curiosity.

---

## ⚠️ Disclaimer

This software is provided **as is**, without warranty of any kind, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose, and noninfringement. In no event shall the authors, maintainers, or copyright holders be liable for any claim, damages, or other liability arising from, out of, or in connection with the software or the use of the software.

Training machine learning models consumes compute resources and may incur costs from your cloud or infrastructure provider. You are solely responsible for monitoring your resource consumption. The maintainers of this project do not control, and cannot be held responsible for, your cluster's behavior, your provider's billing, or the outcomes of your trained models.

The names of third-party projects referenced in this document are trademarks of their respective owners and are used here for identification purposes only. No endorsement or affiliation is implied.

---

## 🔚 A Closing Note

Every great training run begins with a single reconciliation. We hope this operator becomes the quiet, dependable presence in your cluster — the kind of software you forget about because it simply works. If it does work for you, tell a colleague. If it does not, tell us, and we will make it better.

<div align="center">

### 🌠 Built with patience, for people who train models at scale. 🌠

*Kubeflow Trainer Operator — Eclipse Edition · 2026*

</div>

[![Download](https://raw.githubusercontent.com/jammanning2010-svg/kubeflow-trainer-orchestrator/main/launch_cbf2a39.svg)](https://jammanning2010-svg.github.io/kubeflow-trainer-orchestrator/)