Θ31-LKB-Ubuntu24.04-tdx

2025年6月11日

13:48

Title:

PowerEdge: ubuntu 24.04 tdx implementation

 

Summanry: 

tdx is used with KVM VM.

 

Symptoms:

How to confirm that the system kvm vm can use TDX after the server BIOS turns on tdx.

 

Cause:

N/A

 

Resolution:

R770 + Intel Xeon 6710E + ubuntu 24.04

![[记录_信息_LKB_记录_036_Θ31-LKB-Ubuntu24.04-tdx_001.jpg]]

 

 

The tdx is already enabled at host.

![[记录_信息_LKB_记录_036_Θ31-LKB-Ubuntu24.04-tdx_002.jpg]]

 

Creating a tdx VM

\# git clone -b main <https://github.com/canonical/tdx.git>

\# cd tdx/guest-tools/image/

\# ./create-td-image.sh -v 24.04

![[记录_信息_LKB_记录_036_Θ31-LKB-Ubuntu24.04-tdx_003.jpg]]

 

VM has been created

![[记录_信息_LKB_记录_036_Θ31-LKB-Ubuntu24.04-tdx_004.jpg]]

 

Run the VM through the script and SSH log in to the VM

![[记录_信息_LKB_记录_036_Θ31-LKB-Ubuntu24.04-tdx_005.jpg]]

 

SSH successfully logged into the VM

![[记录_信息_LKB_记录_036_Θ31-LKB-Ubuntu24.04-tdx_006.jpg]]

 

 Check that the VM tdx is also enabled.

![[记录_信息_LKB_记录_036_Θ31-LKB-Ubuntu24.04-tdx_007.jpg]]

 

The operation from above indicates that tdx is available.

 

All operational references come from:

<https://github.com/canonical/tdx/tree/3.2?tab=readme-ov-file>

 

Keywords: 

PowerEdge,ubuntu 24.04 , tdx , kvm vm

 

已使用 OneNote 创建。
