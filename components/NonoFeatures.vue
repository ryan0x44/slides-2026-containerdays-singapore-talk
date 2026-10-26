<template>
  <div class="nono-features">
    <div class="hero">
      <img class="hero-logo" src="/logos/nono.svg" alt="nono" />
      <div class="hero-wrap">
        <span class="wrap-label"><carbon-locked />Landlock</span>
        <span class="hero-chip"><carbon-bot />agent</span>
        <span class="hero-chip hero-chip-child"><carbon-parent-child />child processes</span>
      </div>
    </div>

    <div class="features">
      <section class="feature">
        <div class="feature-icon"><carbon-folder-off /></div>
        <h3>Files &amp; subprocesses</h3>
        <p>Restrict access to permitted workspace files. Deny access to agent identity client certificate. Child processes inherit restrictions.</p>
      </section>
      <section class="feature">
        <div class="feature-icon"><carbon-network-3 /></div>
        <h3>Network &amp; credentials</h3>
        <p>Outbound requests are proxied: set policies for allow/deny/HITL approval, inject credentials and custom headers.</p>
      </section>
      <section class="feature">
        <div class="feature-icon"><carbon-task-approved /></div>
        <h3>Human-in-the-loop (HITL) approval</h3>
        <p>Pause network requests, file access or commands until a human approves. Decide in the terminal or via webhooks.</p>
      </section>
      <section class="feature">
        <div class="feature-icon"><carbon-report /></div>
        <h3>Isolated audit logs</h3>
        <p>Operations are logged by the nono supervisor. Store them where agents can't modify them.</p>
      </section>
    </div>

    <div class="today">
      <img src="/logos/gvisor.svg" alt="" />
      <span><strong>Note:</strong> gVisor lacks Landlock (issue #13439) & seccomp user notification, so nono file/command HITL doesn't work yet.</span>
    </div>
  </div>
</template>

<style scoped>
.nono-features {
  display: grid;
  gap: 14px 16px;
  grid-template-columns: 220px minmax(0, 1fr);
  margin-top: 22px;
}

.hero {
  align-items: center;
  background: linear-gradient(160deg, #fff 0%, var(--podplane-surface) 100%);
  border: 1px solid #d7deea;
  border-radius: 16px;
  display: flex;
  flex-direction: column;
  gap: 18px;
  justify-content: center;
  padding: 18px 16px;
}

.hero-logo {
  width: 150px;
}

.hero-wrap {
  border: 2px dashed var(--podplane-blue);
  border-radius: 12px;
  display: flex;
  flex-direction: column;
  gap: 6px;
  padding: 16px 12px 12px;
  position: relative;
  width: 100%;
}

.wrap-label {
  align-items: center;
  background: var(--podplane-blue);
  border-radius: 999px;
  color: #fff;
  display: inline-flex;
  font-size: 10px;
  font-weight: 700;
  gap: 4px;
  left: 50%;
  letter-spacing: 0.08em;
  padding: 2px 9px;
  position: absolute;
  text-transform: uppercase;
  top: -10px;
  transform: translateX(-50%);
}

.hero-chip {
  align-items: center;
  background: #fff;
  border: 1px solid #d7deea;
  border-radius: 8px;
  color: var(--podplane-ink);
  display: flex;
  font-size: 12px;
  font-weight: 700;
  gap: 6px;
  justify-content: center;
  padding: 5px 8px;
}

.hero-chip svg {
  color: var(--podplane-blue);
  font-size: 14px;
}

.hero-chip-child {
  color: var(--podplane-muted);
  font-weight: 600;
  margin-left: 18px;
}

.features {
  display: grid;
  gap: 12px;
  grid-template-columns: 1fr 1fr;
}

.feature {
  background: var(--podplane-surface);
  border: 1px solid #d7deea;
  border-radius: 14px;
  display: grid;
  align-content: start;
  gap: 8px 10px;
  grid-template-columns: auto 1fr;
  padding: 14px 16px;
}

.feature-icon {
  align-items: center;
  background: #fff;
  border: 1.5px solid #d7deea;
  border-radius: 9px;
  color: var(--podplane-blue);
  display: flex;
  font-size: 20px;
  height: 36px;
  justify-content: center;
  width: 36px;
}

.slidev-layout .feature h3 {
  align-self: start;
  color: var(--podplane-blue);
  font-size: 20px;
  margin-top: 3px !important;
  font-weight: 700;
  letter-spacing: -0.025em;
  line-height: 1.2;
  margin: 0;
  opacity: 1;
  text-transform: none;
}

.feature p {
  color: var(--podplane-ink);
  font-size: 13px;
  grid-column: 1 / -1;
  line-height: 1.45;
  margin: 0;
}

.today {
  align-items: center;
  background: #fff7ed;
  border: 1px solid #fed7aa;
  border-left: 4px solid #f97316;
  border-radius: 10px;
  color: var(--podplane-muted);
  display: flex;
  font-size: 15px;
  gap: 10px;
  grid-column: 1 / -1;
  padding: 9px 14px;
}

.today img {
  height: 24px;
  width: 24px;
}

.today strong {
  color: var(--podplane-ink);
}
</style>
