# The Linux Kernel in AOSP

## Android Common Kernel (ACK)

* ACK is downstream of [kernel.org](https://kernel.org)
* The difference between ACK and upstream kernel is that ACK contains just what is needed to run an Android stack

### Old Approach (non-GKI)

```
LTS kernel ==== + ===================================================>
(kernel.org)    |                  |                     |
                V   v v       v    |  v    v    v v  v   |   v      v
               ACK ==== + ======== m =================== m ==========>
                        |                          |
                        V      v    v  vv     vvv  |    v         v
                      Vendor ==== + ============== m ================>
                      kernel      |         |
                                  V         |   v    v       v
                               Product ==== m =======================>
                               kernel
```

```
clone: +     merge: |     cherry-pick: v
       |            |
       V            m
```

* ACK = LTS + Android-specific patches
* Vendor = ACK + SOC-specific patches
* Product = Vendor + product-specific patches

The cost of maintenance increases due to kernel fragmentation.

### New Approach (GKI)

* GKI stands for Generic Kernel Image
* GKI was started since 2019

```
AOSP                                     Vendor
+-----------+       +-------------------------+
| Android   |<----->| HAL                     |
| Framework |       | Implementation          |
+-----------+       +-------------------------+
     |       /-----/   |                 |
=====|======/==========|=================|=====
     |     /           |                 |
+---------+       +---------+       +---------+
| Generic |<----->| GKI     |<----->| Vendor  |
| Kernel  |       | Modules |  KMI  | Modules |
+---------+       +---------+       +---------+
AOSP              AOSP              Vendor
```

All the AOSP blocks are protected, vendors can only modify HAL and vendor modules.

## GKI Mixed Build

In progress ...

## Reference

* [YouTube: The Linux Kernel in AOSP](https://youtu.be/6O878RYYM18?feature=shared)
* [AOSP: The Generic Kernel Image](https://source.android.com/docs/core/architecture/kernel/generic-kernel-image)
* [AOSP: Kernel overview](https://source.android.com/docs/core/architecture/kernel)
