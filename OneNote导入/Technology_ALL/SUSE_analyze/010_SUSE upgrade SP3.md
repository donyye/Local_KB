SUSE upgrade SP3

Wednesday, August 06, 2014

10:19 AM

-  

  URL：https://www.suse.com/support/kb/doc.php?id=7012368&path=29&number=f.1

  Update via using a SLE 11 SP3 installation media

  Please obtain the ISO images from [http://download.novell.com](http://download.novell.com/?ref=suse).

   

  3) Update by booting from a SLES/SLED 11 SP3 media

  To start the standard update via DVD, reboot the computer with this medium in it\'s DVD drive. Perform a system update instead of a fresh installation. To achive this, select\"Installation\" -\> Select language and keyboard layout -\> Agree to the License -\>  Select \"Update an Existing System\" instead of \"New Installation\".

   

  3.1) Update by booting off a SP3 network installation source

  It is also possible to provide the installation media via network. The SLE 11 Service-Pack 3 media contains a complete product. So it can be added to an installation server in the same way as every other SUSE LINUX Enterprise product. The procedure on how to setup an installation server and on how to add the service pack is described in the product documentation. For SLES 11 have a look into chapter 14.2 of the deployment guide. The document is available online under [http://www.suse.com/documentation/sles11/](http://www.suse.com/documentation/sles11/). 

  To start the update, go ahead as follows:

  - A bootable medium is needed to initialize the process. Booting via network/PXE is also possible. For PXE boot configuration examples see chapter 14.3 in the SLES 11 deployment guide (online available at[http://www.suse.com/documentation/sles11/](http://www.suse.com/documentation/sles11) ).
  - Boot the machine and choose \"Installation\".
  - Change the installation source via the \"F4\" key and enter the IP and path to the installation source or select \"SLP\" if this protocol is configured on your installation server.
  - Select \"System Update\" instead of performing a \"New Installation\". 

   

   

  ![Machine generated alternative text: 卿自 ...lesesesesesesesesJ口口 「＼兮户气 ＼澎｝ Enterprise preparation 卜Welcome SystemAnalysis TimeZone Installation 里anguage 一握亘亘i宜五i互动 \~．二妇 夕yboardLayout 一印9lish(us) 1，... ．飞」 SeFVeFSCenaFio InstallationSummary perf0Fm!nst日11日tion Con月guration CheckInstal!ation HOStn日me NetWOFk CUStomeFCenteF onlineUPdate S6FVICe CleanUp ReleaseNotes HardwareCon行guration 、气七、 LicenseAgreement SUSE(R)L主nuxEnterpriseServerforSAPApplications11SP3 SUSESoftwareL工censeAgreement PLEASEREADTHISAGREEMENTCAREFULLY．日丫PURCHASING,INSTALLING AND/ORUSINGTHESOFI侧ARE(INCLUDINGITSCOMPONE研S),YOUAGREETO THETER州5OFTHISAGREEHENTANDACKN《荆LEDGETHATYOUHAVEREADAND UNDERSTANDTHISAGREEMENT.IFYOUDONOTAGREE刊ITHTHESETER州S,DO NOTD《荆NLOAD,INSTALLORUSETHESOFI刊ARE.ANINDIVIDUALACTINGON BEHALFOFANENTITYREPRESENTSTHATHEORSHEHASTHEAUTHORITYTO E阴．ERINTOTHISAGREEMENTONBEHALFOFTHATENTITY. ThiSSUSESOftvareLicenseAgreement(\"Agreement\")15alegal agreementbetweenYOu(anentityOraperSOn)andSUSELLC(\"SUSE\") Thesoftwareproductidentified主nthetitleofthisAgreement,its structure,organization,andaccompanyingdacumentation (collectlvelythe\'\'Softvare\")15protectedbythecopyrightlawsand treatiesoftheUn几tedStatesandothercountriesand15sub\]ectto thetermsofthisAgreement.Anymodification,update,enhancement OrupgradetotheSoft丫arethatYOumaydownloadOrreceivethatare notaccompanledbyaSUSEsoftwarelicenseagreementexpresSly15 includedasSoft丫areandgovernedbythisAgreement. LlcENSEs.TheSoftwareandeachofitscomponents ovnedby 。．。＋卜。．，福。。。。。．。，。月，.0.，。＋。。＋。刁二。闷。．，。。．,. aFe ．卜＋ 1。'a。。．日 SUSE 。＋ko. xl丛greetotheLicense飞rms. LiCenseTFanSlationS二二 Help Ab。。二回 口口口．](attachments/Technology_ALL_SUSE_analyze_010_SUSE%20upgrade%20SP3_001.png)

   

  ![[Technology_ALL_SUSE_analyze_010_SUSE upgrade SP3_002.png]]

   

  ![[Technology_ALL_SUSE_analyze_010_SUSE upgrade SP3_003.png]]

   

  ![[Technology_ALL_SUSE_analyze_010_SUSE upgrade SP3_004.png]]

   

   

  ![[Technology_ALL_SUSE_analyze_010_SUSE upgrade SP3_005.png]]

   

  ![[Technology_ALL_SUSE_analyze_010_SUSE upgrade SP3_006.png]]

   

  ![[Technology_ALL_SUSE_analyze_010_SUSE upgrade SP3_007.png]]

   

  ![[Technology_ALL_SUSE_analyze_010_SUSE upgrade SP3_008.png]]

   

  ![[Technology_ALL_SUSE_analyze_010_SUSE upgrade SP3_009.png]]

   

   

 

已使用 OneNote 创建。
