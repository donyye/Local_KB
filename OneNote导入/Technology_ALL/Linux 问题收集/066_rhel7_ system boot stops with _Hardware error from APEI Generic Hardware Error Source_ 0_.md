rhel7: system boot stops with \"Hardware error from APEI Generic Hardware Error Source: 0\"

2020年8月25日

14:58

![Machine generated alternative text: PowerEdge R73C.:d, root. 1 H. 1 litHapduare Errorl: Harduare error from AFEI Error Smu•ee: \'J B. {I) I Harduare Error\] : seceritg: fatal 0.160046) Error): Error O, fatal Error l; section_tuF: PCIc error Err-or\': port_twe: O. Ele 8.1613%) ErrorJ: version: I .16 Error): : 0\*0007. status: exe010 8.161760) Error): dcuice_id: 0.1619451 Error): slot: O B.te.z«m secondarg&s : OxØ 8.16ZZ5Sl Error): uenaor_id : dwice_id: 8.1624791 Error): c : 000002 8.162647) panic --- Pat-al 8.1628521 Offset: oxzyoeeoo extrrrrrrmtoooooo , in ](attachments/Technology_ALL_Linux%20问题收集_066_rhel7_%20system%20boot%20stops%20with%20_Hardware%20e_001.png)

 

[https://access.redhat.com/solutions/695873](https://access.redhat.com/solutions/695873)

环境

- Red Hat Enterprise Linux (RHEL) 7
- APEI (ACPI Platform Error Interface)

问题

- Trying to boot RHEL7 stops with: \"Hardware error from APEI Generic Hardware Error Source: 0\"
- While RHEL6.5 installs ok on my system, booting RHEL7 ends with this:

[Raw](https://access.redhat.com/solutions/695873#)

[\[Hardware Error\]: Hardware error from APEI Generic Hardware Error Source: 0\
\[Hardware Error\]: APEI generic hardware error status\
\[Hardware Error\]: severity: 1, fatal\
\[Hardware Error\]: section: 0, severity: 1, fatal\
\[Hardware Error\]: flags: 0x01\
\[Hardware Error\]: primary\
\[Hardware Error\]: fru_text: UncorrectedErr\
\[Hardware Error\]: section_type: PCIe error\
\[Hardware Error\]: command: 0x0010, status: 0x0146\
\[Hardware Error\]: device_id: 0000:00:00.5\
\[Hardware Error\]: slot: 0\
\[Hardware Error\]: secondary_bus: 0x00\
\[Hardware Error\]: vendor_id: 0x19a2, device_id: 0x0710\
\[Hardware Error\]: class_code: 000002\
Kernel panic - not syncing: Fatal hardware error!\
Rebooting in 30 seconds..]

决议

- While RHEL6.5 (and older minor versions) evaluate ERST, HEST and EINJ, they do not evaluate the BERT. Booting RHEL7 and hitting above message can be a sign of the BERT containing an error. This might be an error from the past, so the hardware should be tested to ensure that it is not faulty, and then the entry be removed/cleared from the BERT.
- If the hardware is not faulty it may be possible that there is a compatibility issue which will require disabling the Generic Hardware Error Source (GHES) checks. To disable GHES add ghes.disable=1 to your kernel command line from the grub screen to see if the system can boot.
  - For this change be persistent for future reboots, add the ghes.disable=1 to the GRUB_CMDLINE_LINUX in the /etc/default/grub file, and then execute grub2-mkconfig -o /boot/grub2/grub.cfg to rebuild the grub configuration file.

根源

According to ACPI 4.0a Specification, APEI consists of four separate tables:

1.  Error Record Serialization Table (ERST)
2.  BOOT Error Record Table (BERT)
3.  Hardware Error Source Table (HEST)
4.  Error Injection Table (EINJ)

While RHEL6.5 (and older minor versions) evaluate ERST, HEST and EINJ, they do not evaluate the BERT. Booting RHEL7 and hitting above message can be a sign of the BERT containing an error.

 

From \<<https://access.redhat.com/solutions/695873>\>

 

已使用 OneNote 创建。
