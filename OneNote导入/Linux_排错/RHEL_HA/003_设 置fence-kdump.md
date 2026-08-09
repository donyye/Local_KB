设 置fence-kdump

2024年12月4日

11:26

<https://access.redhat.com/solutions/2876971>

 

 

说明：

1）这里的 node-1 和 node-2 是指cluster 里node的名字，使用 pcs status 可以看到。

pcs stonith create kdump fence_kdump pcmk_reboot_action=\"off\" pcmk_host_list=\"node-1 node-2\"

 

 

2）这里的 fence-node-1 or 2 是你的fence device, 比如ipmi/idrac等

\# pcs stonith level add 1 node-1 kdump\
\# pcs stonith level add 1 node-2 kdump\
\# pcs stonith level add 2 node-1 fence-node-1\
\# pcs stonith level add 2 node-2 fence-node-2

 

 

 

 

 

已使用 OneNote 创建。
