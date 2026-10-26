<template>
  <div class="local-risks">
    <div class="laptop">
      <div class="screen">
        <div class="sandbox-row">
          <span class="sandbox-tag"><carbon-security />Sandboxes on your laptop only help with these</span>
          <section class="risk">
            <div class="risk-icon"><carbon-collaborate /></div>
            <div>
              <h3>Agents can interfere<br />with each other</h3>
              <p>Shared git checkouts, database state, ports. Editing tests or bash scripts mid-run.</p>
            </div>
          </section>
          <section class="risk">
            <div class="risk-icon"><carbon-password /></div>
            <div>
              <h3>Agents can exfiltrate sensitive data</h3>
              <p>API &amp; SSH keys, Git creds, personal files. Everything can be read &amp; sent with tool calling.</p>
            </div>
          </section>
        </div>
        <div class="plain-row">
          <section class="risk">
            <div class="risk-icon"><carbon-dashboard /></div>
            <div>
              <h3>Agents compete<br />for laptop resources</h3>
              <p>Concurrent builds, tests, databases, browsers. All consume your CPU, memory, and storage.</p>
            </div>
          </section>
          <section class="risk">
            <div class="risk-icon"><carbon-power /></div>
            <div>
              <h3>Agents stop<br />when your laptop does</h3>
              <p>Event-based agents and remote/multiplayer sessions depend on your laptop staying on.</p>
            </div>
          </section>
        </div>
      </div>
      <div class="base"><span class="notch" /></div>
    </div>

  </div>
</template>

<style scoped>
.local-risks {
  margin-top: 18px;
}

.laptop {
  width: 100%;
}

.screen {
  background: var(--podplane-surface);
  border: 10px solid var(--podplane-ink);
  border-bottom-width: 14px;
  border-radius: 18px 18px 6px 6px;
  box-shadow: 0 14px 30px rgb(18 24 38 / 14%);
  display: grid;
  gap: 22px;
  margin: 0 14px;
  padding: 16px 16px 18px;
}

.base {
  background: linear-gradient(#d7deea, #b8c3d4);
  border-radius: 0 0 14px 14px;
  display: flex;
  height: 12px;
  justify-content: center;
}

.notch {
  background: #9fb0c8;
  border-radius: 0 0 6px 6px;
  height: 5px;
  width: 70px;
}

.sandbox-row,
.plain-row {
  display: grid;
  gap: 14px;
  grid-template-columns: 1fr 1fr;
}

.sandbox-row {
  border: 2px dashed var(--podplane-blue);
  border-radius: 14px;
  padding: 12px 10px 22px;
  position: relative;
}

.plain-row {
  padding: 0 12px;
}

.sandbox-tag {
  align-items: center;
  background: var(--podplane-blue);
  border-radius: 999px;
  color: #fff;
  display: inline-flex;
  font-size: 12px;
  font-weight: 700;
  bottom: -1px;
  gap: 5px;
  left: 50%;
  padding: 3px 11px;
  position: absolute;
  transform: translate(-50%, 50%);
  white-space: nowrap;
}

.risk {
  align-items: center;
  background: #fff;
  border: 1px solid #d7deea;
  border-radius: 12px;
  display: flex;
  gap: 12px;
  min-height: 124px;
  padding: 20px 16px;
}

.risk-icon {
  align-items: center;
  background: #e6f0ff;
  border-radius: 10px;
  color: var(--podplane-blue);
  display: flex;
  flex: 0 0 auto;
  font-size: 20px;
  height: 36px;
  justify-content: center;
  width: 36px;
}

.plain-row .risk-icon {
  background: #fff1e6;
  color: #f97316;
}

.slidev-layout .risk h3 {
  color: var(--podplane-ink);
  font-size: 22px;
  font-weight: 700;
  letter-spacing: -0.025em;
  line-height: 1.2;
  margin: 0 0 6px;
  opacity: 1;
  padding-top: 0;
  text-transform: none;
}

.risk p {
  color: var(--podplane-muted);
  font-size: 13px;
  line-height: 1.4;
  margin: 0;
}
</style>
