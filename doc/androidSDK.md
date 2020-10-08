# Android SDK


## project SDK version Instruct  
1. SDK 最低版本

    可以支持的设备SDK最低版本 

2. SDK 目标版本 

    应用是设计给那个API级别去运行的。大多数情况下，目标版本即为最新发布的安卓版本 


3. SDK 编译版本 

    编译目标制定具体要使用的系统版本 

可通过修改`build.gradle` 文件设置以上3个参数 

##  如何安全添加新版本API代码 

对于不支持旧版本的SDK API，应在程序中添加逻辑判断，判断当前API版本是否可支持新版SDK，如果支持，则使用，否则不使用。 



