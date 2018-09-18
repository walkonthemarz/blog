##### 使用VBS添加windows注册表：
* Google了下，主要参考了两个链接，一下大部分代码来源于参考链接[https://blogs.technet.microsoft.com/heyscriptingguy/2006/11/16/how-can-i-create-a-new-registry-key/](https://blogs.technet.microsoft.com/heyscriptingguy/2006/11/16/how-can-i-create-a-new-registry-key/), [https://stackoverflow.com/questions/17466681/how-to-run-vbs-as-administrator-from-vbs](https://stackoverflow.com/questions/17466681/how-to-run-vbs-as-administrator-from-vbs)

**注意**: VBS默认无法运行带BOM 的**utf8**编码脚本，如果报**800a0408**的代码错误，可以使用自带的记事本将下列代码保存为**ANSI**编码格式即可
```
'AddAutoRunProgram.vbs

'下面一段用于以管理员身份运行当前脚本
Set WshShell = WScript.CreateObject("WScript.Shell")
If WScript.Arguments.Length = 0 Then
  Set ObjShell = CreateObject("Shell.Application")
  ObjShell.ShellExecute "wscript.exe" _
    , """" & WScript.ScriptFullName & """ RunAsAdministrator", , "runas", 1
  WScript.Quit
End if


'假设该程序在c:\mydir文件夹中，文件名为start.vbs

Const HKEY_LOCAL_MACHINE = &H80000002

strComputer = "."

Set objRegistry = GetObject("winmgmts:\\" & strComputer & "\root\default:StdRegProv")

strKeyPath = "Software\Microsoft\Windows\CurrentVersion\Run"

strValueName = "mykeyname"

strValue = "C:\mydir\start.vbs"

objRegistry.SetStringValue HKEY_LOCAL_MACHINE, strKeyPath, strValueName, strValue

'在启动组中添加自启动程序start.vbs

MsgBox("开机启动添加成功！Success!")
```
