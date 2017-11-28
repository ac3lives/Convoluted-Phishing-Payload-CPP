# Convoluted-Phishing-Payload-CPP-
Batch file code for the convoluted phishing payload blog post at www.acenyethehackerguy.com

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
