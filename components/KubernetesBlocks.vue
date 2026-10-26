<template>
  <div class="k8s-blocks">
    <section class="block block-tl">
      <div class="block-icon"><carbon-kubernetes-pod /></div>
      <h3 style="font-size: 85%;">Pods per-agent:<br />their own ports &amp; storage</h3>
      <p>Each Pod gets a network namespace, persistent volume, and sidecars for databases/services.</p>
    </section>

    <section class="block block-tr">
      <div class="block-icon"><carbon-locked /></div>
      <h3 style="font-size: 85%;">Container isolation &amp; access controls</h3>
      <p>Agents can only access what you explicitly allow via service accounts, RBAC, NetworkPolicy, security contexts, RuntimeClasses, and more.</p>
    </section>

    <div class="hub">
      <svg class="spokes" viewBox="0 0 160 300" preserveAspectRatio="none" aria-hidden="true">
        <path d="M80 150 C 40 150, 30 70, 0 70" />
        <path d="M80 150 C 120 150, 130 70, 160 70" />
        <path d="M80 150 C 40 150, 30 230, 0 230" />
        <path d="M80 150 C 120 150, 130 230, 160 230" />
      </svg>
      <div class="hub-core"><carbon-logo-kubernetes /></div>
    </div>

    <section class="block block-bl">
      <div class="block-icon"><carbon-chart-cluster-bar /></div>
      <h3 style="font-size: 85%;">Scheduling:<br />limits &amp; autoscaling</h3>
      <p>Requests, limits, and quotas allocate/restrict resources. Auto-scale to many nodes.</p>
    </section>

    <section class="block block-br">
      <div class="block-icon"><carbon-cloud-services /></div>
      <h3 style="font-size: 85%;">Cloud &amp; On-Prem:<br />not your laptop</h3>
      <p>Authorised multiplayer kubectl exec shells. Webhooks and CronJobs for events.</p>
    </section>
  </div>
</template>

<style scoped>
.k8s-blocks {
  display: grid;
  gap: 16px 0;
  grid-template-columns: minmax(0, 1fr) 150px minmax(0, 1fr);
  grid-template-rows: 1fr 1fr;
  margin-top: 26px;
}

.block {
  background: var(--podplane-surface);
  border: 1px solid #d7deea;
  border-radius: 14px;
  display: grid;
  align-content: start;
  gap: 10px 12px;
  grid-template-columns: auto 1fr;
  padding: 24px 20px 16px;
  position: relative;
}

.block-tl { grid-column: 1; grid-row: 1; }
.block-tr { grid-column: 3; grid-row: 1; }
.block-bl { grid-column: 1; grid-row: 2; }
.block-br { grid-column: 3; grid-row: 2; }

.block-icon {
  align-items: center;
  background: #fff;
  border: 1.5px solid #d7deea;
  border-radius: 10px;
  color: var(--podplane-blue);
  display: flex;
  font-size: 22px;
  height: 40px;
  justify-content: center;
  width: 40px;
}

.slidev-layout .block h3 {
  align-self: start;
  margin-top: 3px !important;
  color: var(--podplane-blue);
  font-size: 17px;
  font-weight: 700;
  letter-spacing: -0.025em;
  line-height: 1.2;
  margin: 0;
  opacity: 1;
  text-transform: none;
}

.block p {
  color: var(--podplane-ink);
  font-size: 14px;
  grid-column: 1 / -1;
  line-height: 1.45;
  margin: 0;
}

.hub {
  align-items: center;
  display: flex;
  grid-column: 2;
  grid-row: 1 / span 2;
  justify-content: center;
  position: relative;
}

.spokes {
  height: 100%;
  inset: 0;
  position: absolute;
  width: 100%;
}

.spokes path {
  fill: none;
  stroke: #9fb0c8;
  stroke-dasharray: 4 4;
  stroke-width: 2;
  vector-effect: non-scaling-stroke;
}

.hub-core {
  align-items: center;
  background: #326ce5;
  border: 6px solid #fff;
  border-radius: 50%;
  box-shadow: 0 0 0 1.5px #d7deea, 0 12px 28px rgb(50 108 229 / 30%);
  color: #fff;
  display: flex;
  font-size: 52px;
  height: 96px;
  justify-content: center;
  position: relative;
  width: 96px;
}
</style>
