<template>
  <div class="kernel-controls">
    <div class="controls-top">
      <section class="control-card visibility-card">
        <h2><carbon-search aria-hidden="true" />Visibility</h2>
        <p>What appears to exist: processes, mounts, network, users.</p>
        <code class="mechanism">namespaces</code>
      </section>

      <section class="control-card privileges-card">
        <h2><ph-key aria-hidden="true" />Privileges</h2>
        <p>Which privileged operations are allowed: mount, raw sockets, ptrace.</p>
        <code class="mechanism">capabilities</code>
      </section>

      <div class="gvisor-callout shared-callout">
        <strong>gVisor:</strong> enforced by gVisor Sentry, not the host kernel
      </div>

      <section class="control-card resources-card">
        <h2><carbon-chart-line aria-hidden="true" />Resources</h2>
        <p>How much can be consumed: CPU, memory, I/O, PIDs.</p>
        <code class="mechanism">cgroups</code>
        <div class="gvisor-callout">
          <strong>gVisor:</strong> Sentry exposes resource limits; host kernel enforces them
        </div>
      </section>
    </div>

    <div class="controls-bottom">
      <section class="control-card syscalls-card">
        <h2><carbon-code aria-hidden="true" />Syscalls</h2>
        <p>Allowed kernel calls and arguments.</p>
        <code class="mechanism">seccomp</code>
        <div class="gvisor-callout">
          <strong>gVisor:</strong> enforced by gVisor Sentry, not host kernel; host applies additional filters to gVisor Sentry itself
        </div>
      </section>

      <section class="control-card access-card">
        <h2><carbon-locked aria-hidden="true" />Access policy</h2>
        <p>Which files, paths and ports a process can access.</p>
        <div class="access-methods">
          <div class="access-method">
            <code class="mechanism">Landlock</code>
            <code class="method-scope">runtime + self-applied</code>
            <div class="gvisor-callout">
              <strong>gVisor:</strong> not <em>yet</em> supported (issue #13439)
            </div>
          </div>
          <div class="access-method">
            <code class="mechanism">AppArmor · SELinux</code>
            <code class="method-scope">host-applied</code>
            <div class="gvisor-callout">
              <strong>gVisor:</strong> applied to Sentry, not process
            </div>
          </div>
        </div>
      </section>
    </div>
  </div>
</template>

<style scoped>
.kernel-controls {
  display: grid;
  gap: 14px;
}

.controls-top {
  display: grid;
  gap: 14px;
  grid-template-columns: 1fr 1fr 1.15fr;
  grid-template-rows: 190px;
}

.controls-bottom {
  display: grid;
  gap: 14px;
  grid-template-columns: 330px minmax(0, 1fr);
  grid-template-rows: 202px;
}

.control-card {
  --control-accent: #8c98aa;
  background: var(--podplane-surface);
  border: 1px solid #d7deea;
  border-top: 4px solid var(--control-accent);
  border-radius: 14px;
  display: flex;
  flex-direction: column;
  min-width: 0;
  padding: 14px 16px;
}

.controls-top > .control-card {
  grid-row: 1;
}

.visibility-card {
  --control-accent: #f97316;
  grid-column: 1;
}

.privileges-card {
  grid-column: 2;
}

.resources-card {
  --control-accent: var(--podplane-blue);
  grid-column: 3;
}

.control-card h2 {
  align-items: center;
  display: flex;
  font-size: 18px;
  font-weight: 700;
  gap: 8px;
  letter-spacing: -0.025em;
  line-height: 1.2;
  margin: 0;
}

.control-card h2 svg {
  color: var(--control-accent);
  flex: 0 0 auto;
  font-size: 20px;
}

.control-card p {
  color: var(--podplane-muted);
  font-size: 14px;
  line-height: 1.4;
  margin: 8px 0 9px;
}

.mechanism,
.method-scope {
  background: transparent;
  display: block;
  font-family: var(--slidev-code-font-family);
  font-size: 14px;
  line-height: 1.35;
  padding: 0;
}

.method-scope {
  color: var(--podplane-muted);
  font-size: 13px;
  margin-top: 4px;
}

.gvisor-callout {
  background: #fff;
  border: 1px dashed var(--podplane-blue);
  border-radius: 8px;
  color: #0055cc;
  font-size: 12px;
  line-height: 1.35;
  margin-top: auto;
  padding: 8px 9px;
}

.gvisor-callout strong {
  font-weight: 700;
}

.shared-callout {
  align-self: end;
  grid-column: 1 / 3;
  grid-row: 1;
  margin: 0 17px 15px;
  z-index: 1;
}

.access-methods {
  display: grid;
  flex: 1;
  gap: 12px;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  margin-top: 9px;
}

.access-method {
  display: flex;
  flex-direction: column;
}

.access-method .gvisor-callout {
  min-height: 51px;
}
</style>
