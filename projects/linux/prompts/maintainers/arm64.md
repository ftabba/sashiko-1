+++
title = "ARM64 Architecture Grounding"
paths = ["arch/arm64/", "drivers/irqchip/irq-gic-v3*", "drivers/irqchip/irq-gic-v4*", "drivers/irqchip/irq-gic-v5*", "include/linux/irqchip/arm-gic-v3.h", "include/linux/irqchip/arm-gic-v4.h", "include/linux/irqchip/arm-gic-v5.h", "drivers/iommu/arm/arm-smmu-v3/pkvm/"]
symbols = []
+++
# ARM64 Architecture Grounding: maintainer notes

These notes cover how to treat an ARM architecture detail that the review cannot confirm from the kernel source or a loaded maintainer note. They apply to every arch/arm64/ patch, KVM and the hypervisor included, and to the GIC and pKVM SMMU drivers.

- The reviewer has no ARM Architecture Reference Manual (the ARM ARM). An architectural detail that neither the kernel source nor a loaded note states (a bit position, an exception class or syndrome field, RES0/RES1 polarity, an ordering or synchronization requirement, what traps or routes where, what is guest-owned versus host-owned) is unverified recall. Do not assert it as fact, do not build a finding on it, and do not carry a rule from another execution mode, GIC version or hardware revision onto the code under review.
- The same bar holds for ruling a concern out. A dismissal that rests on an architectural detail ("this bit is RES0 here", "this access cannot trap", "this field is not exposed to the guest") is unverified recall unless the kernel source or a loaded note shows it, and a wrong dismissal hides a real bug.
- Before reasoning about what a raw mask or shift hides or exposes, find the field it covers in arch/arm64/tools/sysreg, which the kernel generates its register definitions from.
- A concern that rests on an architectural detail that neither the kernel source nor a loaded note settles either way is neither dropped nor reported as a bug. Report it once, at the code that depends on it, as a Low-severity question: state that it rests on an architecture detail the review could not confirm, name the detail as a question, and state what the code does if the answer is yes. Such a question is a speculative finding whose open question is the architecture detail itself, and the maintainers want it at Low rather than at the speculative cap of Medium. Raise it only when the code under review depends on the detail, never to ask whether the code is correct in general.
- Never cite an ARM ARM rule identifier, section or page number. The review has no text to cite, and an invented reference reads as evidence.
