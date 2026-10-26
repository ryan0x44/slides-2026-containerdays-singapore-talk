<template>
  <div class="harness-architecture">
    <div class="flow">
      <div class="node client-node">
        <div class="client-icons"><carbon-screen /><carbon-terminal /><carbon-code /></div>
        <strong>Client / UI</strong>
      </div>

      <svg class="flow-arrow" viewBox="0 0 64 20" aria-hidden="true">
        <defs>
          <marker id="harness-head" markerHeight="8" markerWidth="8" orient="auto-start-reverse" refX="7" refY="4" viewBox="0 0 8 8">
            <path d="M0 0 L8 4 L0 8 z" fill="#0066ff" />
          </marker>
        </defs>
        <line marker-end="url(#harness-head)" x1="4" x2="58" y1="10" y2="10" />
      </svg>

      <div class="node harness-node">
        <strong>Harness</strong>
        <div class="harness-parts">
          <span class="harness-tools"><carbon-renew />Loop</span>
          <span class="harness-tools"><carbon-tool-kit />Tools</span>
        </div>
      </div>

      <svg class="fan" viewBox="0 0 72 152" aria-hidden="true">
        <path v-for="y in targetCenters" :key="y" :d="`M2 76 C 36 76, 30 ${y}, 66 ${y}`" marker-end="url(#harness-head)" />
      </svg>

      <div class="targets">
        <div class="target"><carbon-machine-learning-model />Inference API</div>
        <div class="target"><carbon-plug />MCPs</div>
        <div class="target"><carbon-logo-git />Git</div>
        <div class="target"><carbon-object-storage />S3</div>
      </div>
    </div>

    <div class="shapes">
      <section class="shape-card">
        <h3>Single Process CLI</h3>
        <div class="shape-diagram">
          <div class="boundary">
            <span class="boundary-label">process</span>
            <span class="chip chip-ui">UI</span>
            <span class="chip chip-harness">Loop</span>
            <div class="chip-row">
              <span class="chip chip-tools"><carbon-tool-kit />Tools</span>
              <span class="chip chip-tools"><carbon-plug />MCP</span>
            </div>
          </div>
        </div>
        <div class="card-label">Pi, Claude Code CLI/SDK,<br />Codex <code>--no-daemon</code> mode.</div>
      </section>

      <section class="shape-card">
        <h3>Multi-Client: Server Tools</h3>
        <div class="shape-diagram">
          <div class="client-stack">
            <div class="boundary">
              <span class="boundary-label">client</span>
              <span class="chip chip-ui">UI</span>
            </div>
            <div class="boundary">
              <span class="boundary-label">client</span>
              <span class="chip chip-ui">UI</span>
            </div>
          </div>
          <carbon-arrows-horizontal class="link" />
          <div class="boundary">
            <span class="boundary-label">server</span>
            <span class="chip chip-harness">Loop</span>
            <div class="chip-row">
              <span class="chip chip-tools"><carbon-tool-kit />Tools</span>
              <span class="chip chip-tools"><carbon-plug />MCP</span>
            </div>
          </div>
        </div>
        <div class="card-label">OpenCode server executes tools.<br />Codex (default) daemon mode does too.</div>
      </section>

      <section class="shape-card">
        <h3>Separate Tool Executor</h3>
        <div class="shape-diagram shape-diagram-split">
          <div class="boundary">
            <span class="boundary-label">client</span>
            <span class="chip chip-ui">UI</span>
          </div>
          <carbon-arrows-horizontal class="link" />
          <div class="boundary">
            <span class="boundary-label">server</span>
            <span class="chip chip-harness">Loop</span>
          </div>
          <carbon-arrows-horizontal class="link" />
          <div class="boundary">
            <span class="boundary-label">executor</span>
            <span class="chip chip-tools"><carbon-tool-kit />Tools</span>
            <span class="chip chip-tools"><carbon-plug />MCP</span>
          </div>
        </div>
        <div class="card-label">Codex can split the TUI, app-server (LLM calls), and exec-server (MCP & tool calls)</div>
      </section>
    </div>
  </div>
</template>

<script setup lang="ts">
// Vertical centres of the four target pills (32px tall, 8px apart).
const targetCenters = [16, 56, 96, 136]
</script>

<style scoped>
.harness-architecture {
  display: grid;
  gap: 22px;
  margin-top: 22px;
}

/* Flow: client -> harness -> outbound services */
.flow {
  align-items: center;
  display: grid;
  grid-template-columns: 150px 64px 270px 72px 190px;
  justify-content: center;
}

.node {
  align-items: center;
  border-radius: 16px;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.client-node {
  background: #fff;
  border: 1.5px solid #d7deea;
  box-shadow: 0 6px 16px rgb(18 24 38 / 7%);
  gap: 8px;
  height: 104px;
}

.client-node strong {
  font-size: 17px;
}

.client-icons {
  color: var(--podplane-blue);
  display: flex;
  font-size: 20px;
  gap: 8px;
}

.flow-arrow {
  height: 20px;
  width: 64px;
}

.flow-arrow line,
.fan path {
  fill: none;
  stroke: var(--podplane-blue);
  stroke-width: 2;
}

.harness-node {
  background: var(--podplane-blue);
  box-shadow: 0 10px 24px rgb(0 102 255 / 24%);
  color: #fff;
  gap: 8px;
  height: 152px;
}

.harness-node strong {
  font-size: 26px;
  letter-spacing: -0.03em;
}

.harness-parts {
  display: flex;
  gap: 8px;
}

.harness-tools,
.harness-online {
  align-items: center;
  display: inline-flex;
  gap: 6px;
}

.harness-tools {
  background: rgb(255 255 255 / 16%);
  border: 1px solid rgb(255 255 255 / 35%);
  border-radius: 999px;
  font-size: 15px;
  font-weight: 600;
  padding: 4px 12px;
}

.harness-online {
  font-size: 13px;
  opacity: 0.85;
}

.fan {
  height: 152px;
  width: 72px;
}

.targets {
  display: grid;
  gap: 8px;
  grid-auto-rows: 32px;
}

.target {
  align-items: center;
  background: #fff;
  border: 1.5px solid #d7deea;
  border-radius: 10px;
  display: flex;
  font-size: 15px;
  font-weight: 600;
  gap: 8px;
  padding: 0 12px;
}

.target svg {
  color: var(--podplane-blue);
  font-size: 17px;
}

/* Three architecture shapes */
.shapes {
  display: grid;
  gap: 14px;
  grid-template-columns: repeat(3, minmax(0, 1fr));
}

.shape-card {
  background: var(--podplane-surface);
  border: 1px solid #d7deea;
  border-radius: 14px;
  display: flex;
  flex-direction: column;
  padding: 12px 16px 12px;
}

.slidev-layout .shape-card h3 {
  color: var(--podplane-blue);
  font-size: 18px;
  font-weight: 700;
  letter-spacing: -0.025em;
  line-height: 1.2;
  margin: 0;
  opacity: 1;
  text-transform: none;
}

.shape-diagram {
  align-items: center;
  display: flex;
  flex: 1;
  gap: 10px;
  justify-content: center;
  padding: 14px 0 4px;
}

.boundary {
  align-items: stretch;
  border: 1.5px dashed #9fb0c8;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  gap: 5px;
  min-width: 78px;
  padding: 12px 8px 8px;
  position: relative;
}

.boundary-label {
  background: var(--podplane-surface);
  color: var(--podplane-muted);
  font-size: 10px;
  font-weight: 600;
  left: 8px;
  letter-spacing: 0.06em;
  padding: 0 4px;
  position: absolute;
  text-transform: uppercase;
  top: -8px;
}

.client-stack {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.chip {
  align-items: center;
  border-radius: 6px;
  display: flex;
  font-size: 12px;
  font-weight: 700;
  gap: 4px;
  height: 22px;
  justify-content: center;
  padding: 0 8px;
}

.chip-ui {
  background: #fff;
  border: 1px solid #d7deea;
  color: var(--podplane-ink);
}

.chip-harness {
  background: var(--podplane-blue);
  color: #fff;
}

.chip-tools {
  background: #e6f0ff;
  border: 1.5px solid var(--podplane-blue);
  color: var(--podplane-blue);
}

.link {
  color: var(--podplane-blue);
  flex: 0 0 auto;
  font-size: 22px;
}

.chip-row {
  display: flex;
  gap: 4px;
}

.chip-row .chip {
  padding: 0 6px;
}

.client-stack .boundary {
  min-width: 0;
}

.shape-diagram-split {
  gap: 4px;
}

.shape-diagram-split .boundary {
  min-width: 0;
  padding: 12px 6px 6px;
}

.shape-diagram-split .chip svg {
  flex-shrink: 0;
}

.shape-diagram-split .link {
  font-size: 18px;
}

.card-label {
  border-top: 1px solid #d7deea;
  color: var(--podplane-muted);
  font-size: 13px;
  margin-top: 10px;
  padding-top: 8px;
}
</style>
