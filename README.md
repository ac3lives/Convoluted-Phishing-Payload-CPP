# Convoluted Phishing Payload CPP
Batch file code (moved to txt, see update) for the convoluted phishing payload blog post at http://www.acenyethehackerguy.com/2017/11/convoluted-phishing-payload.html

#### How this payload works (I dub thee name 'CPP' - convoluted phishing payload):
1. Delivered through a Microsoft Word document using Object Linking and Embedding (OLE)
2. OLE is a batch file disguised with another Microsoft product icon (Internet Explorer = win).
3. The batch file does the following:
   * Locates CSC.exe on the machine 
   * Locates system.management.assembly.dll
   * Echos C# code into a text file which..
   * Uses system.management.assembly.dll to run PowerShell code without PowerShell.exe
   * Contains a PowerShell Empire stager
   * Compiles C# using CSC
   * Runs the resulting executable

January 2018 - Updated code, need to update blog post. Notes to update:
  - OLE is dead as of creators update, use this with DDE
  - For DDE, download the txt file using CertUtil, then use 'cmd < cvp.txt' to execute it
  - Changed execution of the EXE to use InstallUtil.exe, bypassing application whitelisting and drawing less scrutiny from AV for executing an unsigned library.
  (Basically, took a bunch of Casey Smith's work on app whitelist bypassing [@subtee] and made it into batch script that echos and compiles locally)
