# Equinox Kernel — edición backslashxx

**Equinox Kernel** es el kernel de **[LF52406](https://github.com/LF52406)** para el
POCO F5 Pro / Redmi K60 (`mondrian`): https://github.com/LF52406/Equinox-Kernel-sm8450

Esta es una modificación de Equinox 5.10.269 hecha por **Nanairo**, con permiso de LF52406.
Todo el trabajo del kernel base (Linux 5.10.269, config de mondrian, DroidSpaces, arreglos
del touch Goodix y la comprobación de KMI) es de LF52406. Si te gusta, apoya al
desarrollador original: https://www.paypal.me/LF52406 · https://patreon.com/InfernalGT ·
https://boosty.to/infernalgt/donate

Los problemas de esta edición repórtalos a Nanairo, no a LF52406 ni a backslashxx.

## Qué cambia respecto de Equinox oficial

- **backslashxx/KernelSU** v3.3.1+55 (UAPI 6) en lugar de KernelSU-Next, integrado en el
  kernel con su propio backend de hooks (hook de la tabla de syscalls,
  `KSU_TAMPER_SYSCALL_TABLE`, compatible con Clang CFI). Usa el manager oficial de backslashxx (paquete `me.weishu.kernelsu`).
- **SUSFS v2.4.0** desde la rama `gki-android12-5.10` de simonpunk, con una interfaz escrita
  para esta edición (backslashxx no trae soporte de SUSFS de fábrica), y todas las funciones
  activas (sus_path, sus_mount, sus_kstat, sus_map, spoof uname, spoof cmdline/bootconfig,
  open_redirect, log y ocultación de símbolos).
- **BBR** como control de congestión TCP por defecto (Equinox trae BBR, pero usa CUBIC
  por defecto).
- **NoMount v2.3.1** de maxsteeel integrado en el kernel. Esta edición hace que las apps a
  las que SUSFS desmonta los módulos vean el sistema original, y oculta NoMount de
  `/sys/module` y `/proc/kallsyms`.
- SUSFS y NoMount se controlan desde el módulo root `ksu_module_susfs` (trae su propio
  binario `ksu_susfs`) y el metamódulo NoMount, ambos instalables desde el manager de
  backslashxx como cualquier módulo de KernelSU — no hace falta un manager especial.
- Compilado con Clang de AOSP y ThinLTO (Equinox usa Neutron Clang y LTO completo). Clang
  CFI y MODVERSIONS siguen activos.

Todo lo demás es Equinox tal cual, incluidos DroidSpaces y las reubicaciones KABI de
SYSVIPC/POSIX_MQUEUE.

## Créditos

- **[LF52406](https://github.com/LF52406)**: Equinox Kernel, la base de todo esto.
- [LineageOS](https://github.com/LineageOS/android_kernel_xiaomi_sm8450): kernel base de mondrian.
- [Android Common Kernel](https://android.googlesource.com/kernel/common) y [Linux](https://www.kernel.org/).
- [backslashxx/KernelSU](https://github.com/backslashxx/KernelSU): root en el kernel, fork de
  [KernelSU](https://github.com/tiann/KernelSU) de **tiann** (weishu).
- [SUSFS](https://gitlab.com/simonpunk/susfs4ksu) de **simonpunk**.
- [DroidSpaces](https://github.com/ravindu644/Droidspaces-OSS) de **ravindu644**.
- [NoMount](https://github.com/maxsteeel/nomount) de **maxsteeel**: redirección de rutas en
  VFS (v2.3.1); incluye el arreglo `7e7342c` de **deref3046**.
- [AnyKernel3](https://github.com/osm0sis/AnyKernel3) de **osm0sis**.
- **[Nanairo8890](https://github.com/nanairo8890)**: integración de backslashxx/KernelSU, SUSFS, BBR y NoMount en Equinox.

---

# Equinox Kernel — backslashxx edition (English)

Equinox Kernel is **LF52406**'s kernel for mondrian. This build is Equinox 5.10.269 modified
by Nanairo with LF52406's permission: **backslashxx/KernelSU** v3.3.1+55 (a fork of
KernelSU) replaces KernelSU-Next and is built in, with a SUSFS v2.4.0 interface written for
this edition (backslashxx ships no SUSFS support of its own), TCP defaults to BBR, and
maxsteeel's NoMount v2.3.1 (which includes deref3046's fix 7e7342c) is built in. SUSFS and
NoMount are controlled from their own root modules (`ksu_module_susfs` and the NoMount
metamodule), installable from backslashxx's own manager like any KernelSU module — no special
manager is needed. Everything else is Equinox as released, DroidSpaces included. All base
kernel work is LF52406's; please report issues with this edition to Nanairo, not to LF52406
or backslashxx.
