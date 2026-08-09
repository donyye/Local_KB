B: 第2题：\"附加题\"使用timesync系统角色

2025年2月6日

11:13

\-\--

\- name: partition

  hosts: balancers

  tasks:

  - name: Create a directory1

[  ][  ]ansible.builtin.file:

[    ][  ]name: /newpart1

[    ][  ]state: directory

  - name: Create a directory2

    ansible.builtin.file:

      name: /newpart2

      state: directory

  - block:

    - name: device vdc 1500M

      community.general.parted:

        device: /dev/vdc

        number: 1

        state: present

        part_end: 1500MiB

    - name: ext4 vdc filesystem

      community.general.filesystem:

        fstype: ext4

        dev: /dev/vdc1

    - name: mount vdc

      ansible.posix.mount:

        path: /newpart2

        src: /dev/vdc1

        fstype: ext4

        state: mounted

 

\- name: device vdb 1500M

      community.general.parted:

        device: /dev/vdb

        number: 1

        state: present

        part_end: 1500MiB

    - name: ext4 vdb filesystem

      community.general.filesystem:

        fstype: ext4

        dev: /dev/vdb1

    - name: mount vdb

      ansible.posix.mount:

        path: /newpart1

        src: /dev/vdb1

        fstype: ext4

        state: mounted

 

    rescue:

    - debug:

        msg: Could not create partation of that size

    - name: device vdb 800M

      community.general.parted:

        device: /dev/vdb

        number: 1

        state: present

        part_end: 800MiB

      when: ansible_facts.devices.vdb is defined

    - name: ext4 vdb filesystem

      community.general.filesystem:

        fstype: ext4

        dev: /dev/vdb1

      when: ansible_facts.devices.vdb is defined

    - name: mount vdb

      ansible.posix.mount:

        path: /newpart1

        src: /dev/vdb1

        fstype: ext4

        state: mounted

      when: ansible_facts.devices.vdb is defined

  

\- name: device vdc 800M

      community.general.parted:

        device: /dev/vdc

        number: 1

        state: present

        part_end: 800MiB

      when: ansible_facts.devices.vdc is defined

    - name: ext4 vdc filesystem

      community.general.filesystem:

        fstype: ext4

        dev: /dev/vdc1

      when: ansible_facts.devices.vdc is defined

    - name: mount vdc

      ansible.posix.mount:

        path: /newpart2

        src: /dev/vdc1

        fstype: ext4

        state: mounted

      when: ansible_facts.devices.vdc is defined

  

\- debug:

        msg: Volume group done not exist

      when: ansible_facts.devices.vdd is undefined

 

\[greg@control ansible\]\$ ansible-navigator run partition.yml -m stdout

 

已使用 OneNote 创建。
