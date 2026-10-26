<template>
  <div class="isolation-diagram" role="img" aria-label="Layers of isolation in traditional containers, gVisor containers and a virtual machine">
    <div class="runtime-heading" style="grid-column: 2">
      <strong>Traditional containers</strong>
      <span>runc</span>
    </div>
    <div class="runtime-heading" style="grid-column: 3">
      <strong>gVisor containers</strong>
      <span>runsc · userspace kernel</span>
    </div>
    <div class="runtime-heading" style="grid-column: 4">
      <strong>Virtual machine</strong>
      <span>kata · QEMU, Firecracker, Cloud HV · guest kernel</span>
    </div>

    <div class="layer-boundary app-boundary" style="grid-row: 2" />
    <div class="layer-label app-label" style="grid-row: 2">
      <strong>App/Runtime isolation</strong>
      <span>isolates, Wasm</span>
    </div>
    <div class="layer-box app-box" style="grid-area: 2 / 2">Untrusted code</div>
    <div class="layer-box app-box" style="grid-area: 2 / 3">Untrusted code</div>
    <div class="layer-box app-box" style="grid-area: 2 / 4">Untrusted code</div>

    <div class="layer-boundary kernel-boundary" style="grid-row: 3" />
    <div class="layer-label kernel-label" style="grid-row: 3">
      <strong>Process isolation</strong>
      <span>namespaces, cgroups, capabilities, seccomp</span>
    </div>
    <div class="layer-box kernel-box" style="grid-area: 3 / 2">enforced by host kernel</div>
    <div class="layer-box kernel-box" style="grid-area: 3 / 3; font-size: 0.48em">*enforced by gVisor Sentry</div>
    <div class="layer-box kernel-box" style="grid-area: 3 / 4">enforced by guest kernel</div>

    <div class="layer-boundary kernel-boundary" style="grid-row: 4" />
    <div class="layer-label kernel-label" style="grid-row: 4">
      <strong>Mandatory access control</strong>
      <span>LSM · AppArmor, SELinux</span>
    </div>
    <div class="layer-box policy-box" style="grid-area: 4 / 2">enforced by host kernel</div>
    <div class="layer-box policy-box muted" style="grid-area: 4 / 3">applies to Sentry, not code</div>
    <div class="layer-box policy-box" style="grid-area: 4 / 4">enforced by guest kernel</div>

    <div class="layer-boundary kernel-boundary" style="grid-row: 5" />
    <div class="layer-label kernel-label" style="grid-row: 5">
      <strong>Kernel boundary</strong>
      <span>for process syscalls &amp; I/O</span>
    </div>
    <div class="layer-box host-box" style="grid-area: 5 / 2 / 8 / 3">
      <span>Host kernel<br>full ABI</span>
    </div>
    <div class="layer-box kernel-box" style="grid-area: 5 / 3">
      <span>gVisor Sentry:<br>reimplemented Linux ABI</span>
    </div>
    <div class="layer-box kernel-box" style="grid-area: 5 / 4">Guest kernel</div>

    <div class="layer-boundary" style="grid-row: 6" />
    <div class="layer-label" style="grid-row: 6">
      <strong>Devices / Files</strong>
      <span>drivers · filesystems · network</span>
    </div>
    <div class="layer-box host-box" style="grid-area: 6 / 3 / 8 / 4">
      <span>Host kernel<br>smaller surface</span>
    </div>
    <div class="layer-box policy-box" style="grid-area: 6 / 4">VMM: virtual hardware, virtio</div>

    <div class="layer-boundary" style="grid-row: 7" />
    <div class="layer-label" style="grid-row: 7">
      <strong>CPU / Memory</strong>
      <span>physical hardware</span>
    </div>
    <div class="vm-kernels" style="grid-area: 7 / 4">
      <div class="layer-box host-box"><span>Host kernel<br>Linux + KVM</span></div>
      <div class="layer-box host-box"><span>Hypervisor kernel<br>Xen · ESXi</span></div>
    </div>
  </div>
</template>

<style scoped>
.isolation-diagram {
  --isolation-warm: #a75a08;
  --isolation-line: #a8b3c3;
  display: grid;
  gap: 7px 12px;
  grid-template-columns: 186px 171px 181px minmax(0, 1fr);
  grid-template-rows: 52px 52px 56px 50px 58px 56px 56px;
  isolation: isolate;
  line-height: 1.2;
  padding-right: 7px;
}

.runtime-heading {
  display: flex;
  flex-direction: column;
  gap: 4px;
  grid-row: 1;
  justify-content: flex-end;
  padding: 0 8px;
}

.runtime-heading strong {
  font-size: 15px;
  font-weight: 700;
  letter-spacing: -0.025em;
}

.runtime-heading span {
  color: var(--podplane-muted);
  font-size: 12px;
}

.layer-boundary {
  border: 1.5px dashed var(--isolation-line);
  border-radius: 10px;
  grid-column: 1 / -1;
  margin-right: -7px;
  z-index: -1;
}

.app-boundary {
  border-color: var(--isolation-warm);
}

.kernel-boundary {
  border-color: var(--podplane-blue);
}

.layer-label {
  align-self: center;
  display: flex;
  flex-direction: column;
  gap: 3px;
  grid-column: 1;
  padding: 0 0 0 10px;
}

.layer-label strong {
  font-size: 14px;
  font-weight: 700;
  letter-spacing: -0.025em;
}

.layer-label span {
  color: var(--podplane-muted);
  font-size: 11.5px;
}

.app-label strong {
  color: var(--isolation-warm);
}

.kernel-label strong {
  color: var(--podplane-blue);
}

.layer-box {
  align-items: center;
  align-self: stretch;
  border: 1px solid transparent;
  border-radius: 7px;
  display: flex;
  font-size: 13px;
  margin: 6px 0;
  padding: 5px 9px;
}

.app-box {
  background: #fff2dc;
  border-color: #e8c48c;
  color: var(--isolation-warm);
  font-weight: 700;
}

.kernel-box {
  background: #e7f0ff;
  border-color: #a9caff;
  color: #004ebf;
  font-weight: 700;
}

.policy-box {
  background: #e8edf5;
  border-color: #d7deea;
}

.host-box {
  background: var(--podplane-surface);
  border-color: #d7deea;
  color: var(--podplane-muted);
}

.vm-kernels {
  display: grid;
  gap: 7px;
  grid-template-columns: 1fr 1fr;
}

.vm-kernels .layer-box {
  font-size: 12px;
  padding-inline: 7px;
}
</style>
