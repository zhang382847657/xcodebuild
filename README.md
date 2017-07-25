## **先cd到项目的根目录**
# 第一步：归档生成.xcarchive
> ### 方法一：使用自动签名功能进行归档 
```
xcodebuild -project  <XXX.xcodeproj所在路径>  -archivePath  <XXX.xcarchive所在路径>  -scheme <Scheme名字>  -configuration  <Debug或Release>  -sdk iphoneos archive DEVELOPMENT_TEAM="<TeamID>"
```

_-project   指向xcode工程文件的路径_ <br>
_-archivePath  导出的归档的文件路径_ <br>
_-scheme  项目的scheme名字_ <br>
_-configuration  通常设置Debug或者Release_ <br>
_-DEVELOPMENT_TEAM  苹果开发者团队TEAMID_ 
<br>

> 例子：比如项目的名字叫做CostumSliderProject
```
xcodebuild -project  /Users/zhanglin/练习/costomslider/ios/CostumSliderProject.xcodeproj  -archivePath  /Users/zhanglin/练习/costomslider/ios/CostumSliderProject.xcarchive  -scheme CostumSliderProject  -configuration  Release  -sdk iphoneos archive DEVELOPMENT_TEAM="26429T4LED"
```

<br><br>

> ### 方法二：使用手动添加证书、描述文件进行归档
```
xcodebuild -project <XXX.xcodeproj所在路径>  -archivePath  <XXX.xcarchive所在路径> -scheme <Scheme名字> -configuration <Debug或Release> -sdk iphoneos archive DEVELOPMENT_TEAM="<TeamID>" CODE_SIGN_IDENTITY="<证书名字>" PROVISIONING_PROFILE_SPECIFIER="<描述文件名字>"
```

_-project   指向xcode工程文件的路径_ <br>
_-archivePath  导出的归档的文件路径_ <br>
_-scheme  项目的scheme名字_ <br>
_-configuration  通常设置Debug或者Release_ <br>
_-DEVELOPMENT_TEAM  苹果开发者团队TEAMID_ <br>
_-CODE_SIGN_IDENTITY  证书名字（一定要去钥匙串里复制粘贴正确的证书名,别忘了前缀！！ iPhone Distribution: ****）_<br>
_-PROVISIONING_PROFILE_SPECIFIER  描述文件的名字_

<br>

> 例子：比如项目的名字叫做CostumSliderProject
```
xcodebuild -project /Users/zhanglin/练习/costomslider/ios/CostumSliderProject.xcodeproj -archivePath /Users/zhanglin/练习/costomslider/ios/CostumSliderProject.xcarchive -scheme CostumSliderProject -configuration Release -sdk iphoneos archive DEVELOPMENT_TEAM="26429T4LED" CODE_SIGN_IDENTITY="iPhone Developer: 张 琳 (TYYXE4LBF9)" PROVISIONING_PROFILE_SPECIFIER=“CostumSliderProject"
```

<br><br>


# 第二步：通过.xcarcjove生成.ipa
```
xcodebuild -exportArchive -archivePath <XXX.xcarchive所在路径> -exportPath <导出.ipa的路径>  -exportOptionsPlist <XXX.plist所在路径>
```

_-archivePath  导出的归档的文件路径_ <br>
_-exportPath  想要导出.ipa文件的路径_ <br>
_-exportOptionsPlist  plist文件所在的位置（不是info.plist，这是xcode8.3后不支持-exportFormat命令后，改成用plist文件来支持了）_ <br>


> 例子：比如项目的名字叫做CostumSliderProject
```
xcodebuild -exportArchive -archivePath /Users/zhanglin/练习/costomslider/ios/CostumSliderProject.xcarchive -exportPath /Users/zhanglin/Desktop/CostumSliderProjectIPA  -exportOptionsPlist /Users/zhanglin/练习/costomslider/ios/exportPlist.plist
```


> ##### 测试环境plist文件
```
<?xml version="1.0" encoding="UTF-8"?> 
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
  <dict>
    <key>method</key>
    <string>development<string>
  </dict>
</plist>
```
> ##### 正式环境plist文件
```  
<?xml version="1.0" encoding="UTF-8"?> 
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
   <dict>
     <key>method</key>
     <string>app-store<string>
     <key>teamID</key>
     <string>你的苹果开发者团队ID<string>
   </dict>
</plist> 
```   
[参考文章看这里](http://123.57.28.121/index.php/2016/10/28/code-sign-in-xcode8/)
