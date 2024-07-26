1. 认为是源生代码问题是，先去[https://kmp.jetbrains.com/]下载模板  
2. 打开下载的模板后，按照测试异常是的场景进行填充代码后验证。  
    2.1 先在setting.gradle中添加maven信息，比如开发版的：maven("https://maven.pkg.jetbrains.space/public/p/compose/dev")  
    2.2 在gradle/libs.versions.toml中添加或者修改版本信息：compose-plugin = "1.7.0-dev1743"  
    2.3 如果是桌面端，需要配置 desktopRun。在运行旁边点击“Edit Configurations”，新增 gradle，然后填写名称，并且在run中添加命令 composeApp:desktopRun -DmainClass=MainKt。其中composeApp指的是运行桌面端的app项目  
    2.4 将验证代码补充完成，然后进行运行验证  
3. 如果验证的结果确定有问题，那么就需要去youtrack中处理  
    3.1 先打开youtrack：https://youtrack.jetbrains.com 需要注册账号。可以直接使用谷歌账号。  
    3.2 左边下拉选择，比如我目前是要提交Compose MultiPlatform问题，选中后，可以搜索下我们发现的问题，比如SwingPanel resize会挂掉的问题。  
    3.3 如果有找到，那么就看下具体是否已解决，哪个版本解决了；如果是未解决，那么就可以看下对方验证版本情况是否跟自己一样，表现状态是否一致等，进行自己遇到的问题进行补充，或者自己再建一个也可以。可以手机下载youtracker进行跟踪问题进度。  
    3.4 如果没有找到，那就可以自己创建一个问题，然后手机下载youtracker进行问题跟踪。    
