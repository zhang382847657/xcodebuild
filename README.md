# 目录
- [XCode9.0打包](#XCode9.0打包命令)
- [XCode8.3打包](#XCode8.3打包命令)

## XCode9.0打包命令
### 1. 归档生成.xcarchive

与XCode8.3不同的是，不需要在这一步设置证书和描述文件了，直接进行归档操作

```
xcodebuild archive -workspace ${***.xcworkspace} -scheme ${scheme} -configuration ${buildConfiguration} -archivePath ${***.xcarchive}
```

> - workspace： `xcworkspace`所在的绝对路径，如果是`.xcodeproj`类型的文件，把 `-workspace`替换成`-project`
> - scheme： 项目scheme名字
> - configuration： 通常设置Debug或者Release
> - archivePath： 导出的归档文件的绝对路径


### 2. 生成.ipa

更新到xcode9.0以后，plist文件字段也不一样了，多了`provisioningProfiles`

```  
<?xml version="1.0" encoding="UTF-8"?> 
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
   <dict>
     <key>provisioningProfiles</key>
	<dict>
		<key>项目的Bundle Identifiler</key>
		<string>描述文件的名字</string>
	</dict>
	<key>teamID</key>
	<string>你的苹果开发者团队ID</string>
	<key>method</key>
	<string>包含四种：app-store, ad-hoc, enterprise, development</string>
	<key>compileBitcode</key>
	<false/>
	<key>uploadSymbols</key>
	<false/>
   </dict>
</plist> 
```  

```
xcodebuild -exportArchive -archivePath ${***.xcarchive} -exportPath ${导出ipa的路径} -exportOptionsPlist ${****/exportOptionsPlist.plist}
```

> - archivePath： 上一步归档文件所在的绝对路径
> - exportPath： 要到处ipa的路径
> - exportOptionsPlist： exportOptionsPlist.plist所在的绝对路径


## XCode8.3打包命令
### 1. 归档生成.xcarchive
- 方法一：使用自动签名功能进行归档 
	```
	xcodebuild -project  ${***.xcodeproj}  -archivePath  ${***.xcarchive}  -scheme ${scheme}  -configuration  ${buildConfiguration}  -sdk iphoneos archive DEVELOPMENT_TEAM=${teamId}
	```
	
	> - project   指向xcode工程文件的路径
	> - archivePath  导出的归档的文件路径
	> - scheme  项目的scheme名字
	> - configuration  通常设置Debug或者Release
	> - DEVELOPMENT_TEAM  苹果开发者团队TEAMID



- 方法二：使用手动添加证书、描述文件进行归档
	```
	xcodebuild -project ${***.xcodeproj}  -archivePath  ${***.xcarchive} -scheme ${scheme} -configuration ${buildConfiguration} iphoneos archive DEVELOPMENT_TEAM=${teamId}CODE_SIGN_IDENTITY=${证书名字} PROVISIONING_PROFILE_SPECIFIER=${描述文件名字}
	```
	> - project： 指向xcode工程文件的路径
	> - archivePath： 导出的归档的文件路径
	> - scheme： 项目的scheme名字
	> - configuration： 通常设置Debug或者Release
	> - DEVELOPMENT_TEAM： 苹果开发者团队TEAMID
	> - CODE_SIGN_IDENTITY： 证书名字（一定要去钥匙串里复制粘贴正确的证书名,别忘了前缀！！ iPhone Distribution: ****）
	> - PROVISIONING_PROFILE_SPECIFIER： 描述文件的名字




### 2. 生成.ipa
在生成ipa之前，需要在项目中新增一个plist文件，用来配置打包

```
///////////测试环境///////////
<?xml version="1.0" encoding="UTF-8"?> 
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
  <dict>
    <key>method</key>
    <string>development<string>
  </dict>
</plist>

////////////正式环境/////////////
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

```
xcodebuild -exportArchive -archivePath ${***.xcarchive} -exportPath ${导出ipa的路径}  -exportOptionsPlist ${****/exportOptionsPlist.plist}
```

> - archivePath  导出的归档的文件路径
> - exportPath  想要导出.ipa文件的路径
> - exportOptionsPlist  plist文件所在的位置（不是info.plist，这是xcode8.3后不支持-exportFormat命令后，改成用plist文件来支持了)

  
[参考文章看这里](http://123.57.28.121/index.php/2016/10/28/code-sign-in-xcode8/)
