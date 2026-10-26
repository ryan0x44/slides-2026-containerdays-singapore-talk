---
theme: apple-basic
title: Securing AI Coding Agents with Kubernetes Sandboxes
info: |
  A talk for ContainerDays & AI Context Singapore 2026.
  Licensed under Creative Commons Attribution 4.0 International.
author: Ryan Djurovich
colorSchema: light
download: true
exportFilename: slides
duration: 25min
transition: slide-left
mdc: true
---

<div class="opening-content">
  <div class="question">Securing AI Coding Agents with<br><span class="accent">Kubernetes Sandboxes</span></div>
</div>

---

<script setup>
import LocalRisks from './components/LocalRisks.vue'
</script>

# Running agents locally: <span class="accent">what could go wrong?</span>

<LocalRisks />

---

<script setup>
import KubernetesBlocks from './components/KubernetesBlocks.vue'
</script>

# Kubernetes gives us the building blocks

<KubernetesBlocks />

---

<h1>
  Let's talk about <span class="accent">"Sandboxes"</span>
</h1>

<div class="sandbox-chatter">
  <div class="sandbox-question">Are we talking about…</div>
  <div class="sandbox-bubble sb-vm"><carbon-virtual-machine />VMs?</div>
  <div class="sandbox-bubble sb-containers"><carbon-container-software />Containers?</div>
  <div class="sandbox-bubble sb-kernels"><carbon-chip />Userspace Kernels?</div>
  <div class="sandbox-bubble sb-isolates"><carbon-application-web />Isolates?</div>
  <div class="sandbox-bubble sb-wasm"><carbon-code />WebAssembly?</div>
  <div class="sandbox-bubble sb-agent-sandbox"><img src="/logos/agent-sandbox.svg" alt="" />Kubernetes Agent Sandbox Sandboxes?</div>
</div>

<!--
The answer is: yes.
-->

---
class: isolation-slide
---

<script setup>
import IsolationLayers from './components/IsolationLayers.vue'
</script>

# Sandboxing untrusted code on Kubernetes: <span class="accent">layers of isolation</span>

<IsolationLayers />

---
class: kernel-controls-slide
---

<script setup>
import KernelControls from './components/KernelControls.vue'
</script>

# Process isolation & access control: <span class="accent">what the kernel restricts</span>

<KernelControls />

---

<script setup>
import HarnessArchitecture from './components/HarnessArchitecture.vue'
</script>

# Agent harness architecture: <span class="accent">tools & egress points</span>

<HarnessArchitecture />

---

# Agent harness sandboxes: <span class="accent">isolate secrets, limit access</span>

<div class="trust-map">
  <div class="trust-client">Authenticated client <span>↓ private harness connection</span></div>
  <div class="trust-execution">
    <div class="eyebrow">Agent Sandbox execution boundary</div>
    <div class="trust-inner"><div class="trust-harness"><strong>Harness</strong><span class="trust-chip trust-chip-loop"><carbon-renew />Loop</span><span class="trust-chip"><carbon-tool-kit />Tools</span><span class="trust-chip"><carbon-plug />MCP</span></div><span>nono: restrict files, network and child processes</span></div>
    <div class="trust-workspace">Separate workspace PVC for Git checkout · Separate sidecar services · Identity certificate for inference/git access</div>
  </div>
  <div class="trust-edges"><span>Inference & MCP requests ↓</span><span>Git operations ↓</span></div>
  <div class="talk-grid trust-gateways">
    <div class="talk-card"><h3>Agent Router</h3><p>Caller policy → upstream credential → model API</p></div>
    <div class="talk-card"><h3>Source Gateway</h3><p>Caller policy → upstream credential → Git host</p></div>
  </div>
</div>

<div class="takeaway">The sandbox identity certificate is isolated from the harness. Upstream credentials live outside the sandbox.</div>

---

<h1>Kubernetes Agent Sandbox <span class="accent">for Agent Harness Pods</span></h1>

<div class="talk-grid sandbox-detail">
  <div>

```yaml
apiVersion: agents.x-k8s.io/v1beta1
kind: Sandbox
metadata:
  name: coding-task
spec:
  podTemplate:
    spec:
      runtimeClassName: gvisor
      containers:
        - name: agent
          image: your-agent-image:version
```

  </div>
  <div class="talk-card">
      <h3>Why not a StatefulSet?</h3>
      <p>A StatefulSet runs replica Pods from a spec.</p>
      <p>A Sandbox represents an individual workspace with lifecycle controls; only ever one Pod.</p>
      <h3>Additional Sandbox Benefits</h3>
      <p>Correct PV/PVC handling, explicit suspension, shutdown deadlines, and (optional) warm-pools.</p>
  </div>
</div>

<div class="takeaway">The Agent Sandbox controller schedules exactly one Pod per Sandbox. <strong>The RuntimeClass provides isolation.</strong></div>

---

<h1>Agent Router <span class="accent">for AI Inference & MCP Access</span></h1>

<div class="harness-flow">Agent identity <span>→</span> Policy <span>→</span><strong>Agent Router</strong> <span>→</span> Model provider, MCP Server upstreams</div>

<div class="talk-grid talk-three">
  <div class="talk-card"><h3>Authenticate the caller</h3><p>Enforce agent identity. Configure models & MCP servers agents can use.</p></div>
  <div class="talk-card"><h3>Apply policy centrally</h3><p>Route models; configure request limits, token budgets, and usage telemetry.</p></div>
  <div class="talk-card"><h3>Keep keys upstream</h3><p>The gateway injects provider credentials. Secure and rotate them centrally.</p></div>
</div>

<div class="takeaway">Currently a challenge for model subscriptions. <strong>Provides centralised control, visibility, logging, & auditability.</strong></div>

---

<h1>Source Gateway <span class="accent">for restricted, cached Git & S3 access</span></h1>

<div class="harness-flow">Agent identity <span>→</span> Policy <span>→</span><strong>Source Gateway</strong> <span>→</span> Git & Object Storage upstreams</div>

<div class="talk-grid talk-three">
  <div class="talk-card"><h3>Authenticated, fast checkouts & S3 sync</h3><p>Clone and fetch through a local mirror. Authorise reads even when the cache is warm.</p></div>
  <div class="talk-card"><h3>Explicit write policy, defined centrally</h3><p>Decide which agent identity may write to object storage or git push, to which branch. Prevent accidental deletion.</p></div>
  <div class="talk-card"><h3>Keep Git & Object Store credentials upstream</h3><p>The gateway holds the upstream token or GitHub App credential as a secret and forwards permitted pushes.</p></div>
</div>

<div class="takeaway">Mitigate the "thundering herd" when launching many agents.</div>

---

<script setup>
import NonoFeatures from './components/NonoFeatures.vue'
</script>

<h1>
    Nono <span class="accent">for file/network isolation & HITL with Landlock</span>
</h1>

<NonoFeatures />

---

<h1>
    Demo: <span class="accent">gVisor, Agent Sandbox/Router, Source Gateway</span>
</h1>

<p class="text-sm">Demo & tomorrow's workshop pre-work:</p>

```bash
brew install podplane/tap/podplane
# or: go install github.com/podplane/podplane@latest

podplane deps download
# downloads all dependencies, enabling you to run everything offline

podplane local start
# ~1m30s and you have an Agentic Kubernetes PaaS

kubectl get nodes
kubectl get runtimeclasses
kubectl get pods -A
```

<div class="takeaway"><strong>The demo & workshop</strong>: Pi on Agent Sandbox with gVisor, Agent Router inference, git clone via Source Gateway.</div>

---

# These projects are <span class="accent">open source, Apache 2.0 licensed</span>

<div class="project-cards">
  <div class="project-card-row project-card-row-5">
    <a class="project-card project-card-tile" href="https://agent-sandbox.sigs.k8s.io/">
      <img class="project-logo project-logo-icon project-logo-agent-sandbox" src="/logos/agent-sandbox.svg" alt="" />
      <strong>Agent Sandbox</strong>
      <span class="github-repo">agent-sandbox.sigs.k8s.io</span>
    </a>
    <a class="project-card project-card-tile" href="https://gvisor.dev/">
      <img class="project-logo project-logo-icon" src="/logos/gvisor.svg" alt="" />
      <strong>gVisor</strong>
      <span class="github-repo">gvisor.dev</span>
    </a>
    <a class="project-card project-card-tile" href="https://theagentrouter.ai/">
      <img class="project-logo project-logo-icon" src="/logos/agentrouter.svg" alt="" />
      <strong>Agent Router</strong>
      <span class="github-repo">theagentrouter.ai</span>
    </a>
    <a class="project-card project-card-tile" href="https://sourcegateway.dev/">
      <div class="project-logo project-logo-wordmark">Source<span>Gateway</span></div>
      <span class="github-repo">sourcegateway.dev</span>
    </a>
    <a class="project-card project-card-tile" href="https://nono.sh/">
      <img class="project-logo project-logo-icon project-logo-nono" src="/logos/nono.svg" alt="nono" />
      <span class="github-repo">nono.sh</span>
    </a>
  </div>
  <a class="project-card project-card-featured" href="https://github.com/podplane/podplane">
    <img class="project-logo project-logo-podplane" src="https://podplane.dev/podplane-logo.svg" alt="Podplane" />
    <div class="project-github">
      <strong>Podplane on GitHub</strong>
      <span class="github-repo"><carbon-logo-github />podplane/podplane</span>
    </div>
    <ProjectQr value="https://github.com/podplane/podplane" :size="116" />
  </a>
</div>

<div class="final-contact">
  <span class="text-sm font-bold">Ryan Djurovich:</span>
  <a href="https://x.com/ryan0x44"><carbon-logo-x />@ryan0x44</a>
  <a href="https://github.com/ryan0x44"><carbon-logo-github />ryan0x44</a>
  <a href="https://ryan0x44.com"><carbon-earth />ryan0x44.com</a>
</div>
