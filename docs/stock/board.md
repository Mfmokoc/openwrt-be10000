# Xiaomi BE10000 (RC01) — стоковые данные (подтверждено 2026-11-21)

## Идентификация
- model (bdata/env): RC01
- stock DTB model: Qualcomm Technologies, Inc. IPQ9574/AP-AL02-C4
- compatible: qcom,ipq9574-ap-al02-c4 / qcom,ipq9574-al02 / qcom,ipq9574
- стоковое ядро: Linux 5.4.164 (QSDK), FIT default config = config@al02-c4
- стоковый FIT: kernel@1 (load 0x42080000) + fdt@al02-c4

## Разметка MTD (физические, из /proc/mtd)
| mtd | имя | size | erasesize |
|---|---|---|---|
| 0/1 | 0:SBL1 / 0:SBL1_1 | 0x180000 | 0x20000 |
| 2 | 0:MIBIB | 0x100000 | 0x20000 |
| 3/4 | 0:BOOTCONFIG / 1 | 0x80000 | 0x20000 |
| 5/6 | 0:QSEE / _1 | 0x380000 | 0x20000 |
| 7..16 | DEVCFG/APDP/TME/RPM/CDT (+_1) | 0x80000 | 0x20000 |
| 17 | 0:APPSBLENV | 0x80000 | 0x20000 |
| 18/19 | 0:APPSBL / _1 | 0x180000 | 0x20000 |
| 20 | 0:ART | 0x140000 | 0x20000 |
| 21 | 0:ETHPHYFW | 0x100000 | 0x20000 |
| 22 | 0:TRAINING | 0x80000 | 0x20000 |
| 23 | 0:LICENSE | 0x40000 | 0x20000 |
| 24 | rootfs (слот 0, СТОК) | 0x3c00000 | 0x20000 |
| 25 | rootfs_1 (слот 1, ЦЕЛЬ) | 0x3c00000 | 0x20000 |
| 26 | bdata | 0x40000 | 0x20000 |
| 27 | crash | 0x40000 | 0x20000 |
| 28 | crash_syslog | 0x40000 | 0x20000 |
| 29 | overlay (ubi1, полон) | 0x4600000 | 0x20000 |
| 30..34 | kernel/ubi_rootfs/cfg/user/plugin | gluebi-тома UBI | — |

## U-Boot env
- одна область: offset 0x0, size 0x10000, erase 0x20000 (CRC32 проверен)
- fw_env.config для OpenWrt: `/dev/mtd17 0x0 0x10000 0x20000`
- bootcmd=bootmiwifi, bootdelay=3, bootmenu_delay=5, boot_wait=on
- текущие флаги: boot_rootfs=0 last_success=0 boot_success=1 ota_reboot=0 try_sys1/2_failed=0
- TFTP recovery: ipaddr 192.168.31.1, serverip 192.168.31.100

## Геометрия UBI (для factory.ubi)
- eraseblock 128 KiB, min I/O 2048, LEB 124 KiB
- слот 60 MiB = 480 LEB (~58 MiB полезных), reserved 40, bad 0
- тома стока: kernel (static), ubi_rootfs (dynamic)
- overlay (ubi1) заполнен → rootfs_data OpenWrt размещаем внутри слота 1

## Источник MAC / калибровки
- MAC: nvmem ART (mac-base), см. DTS донора; label-MAC = ethaddr (суффикс SSID 0f0d)
- Wi-Fi caldata/BDF: ART + стоковый rootfs
- разделы в DTS: compatible "qcom,smem-part" (наследуем стоковую MIBIB, без переразметки)

## GPIO (сток ↔ донор совпадают)
- keys: mesh=44, reset=45 (active low)
- LED: sys_white=40, sys_amber=41, net_white=42, net_amber=43, sfp_green=62
- mdio reset=60, aqr113c reset=61
- pcie perst: pcie1=26, pcie2=29, pcie3=23
- sfp: los=46, tx-disable=64
