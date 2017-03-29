# 概述
> 为游戏开发打造的即时通讯SDK（IGIM SDK），让游戏开发者只需调用IGIM SDK提供的接口，即可快速在手游中实现聊天、群组聊天、文字、表情、语音等多项功能。

# 四步极快速集成游戏语音

## 步骤
1. 获取IMLib.framework
2. 获取Appid
3. 导入IMLib,配置工程
4. 调用IMClient提供的接口，实现游戏内即时消息，语音识别等功能

## 具体实施
### 获取IMLib
登录讯飞开放平台 - 游戏解决方案（<http://game.xfyun.cn/>），进入SDK下载，下载IMLib。

### 获取Appid
登录讯飞开放平台 - 游戏解决方案（<http://game.xfyun.cn/>），选择/创建应用并且接入游戏解决方案，进入“控制台”查看对应Appid。

### 导入IMLib至APP工程
1. 创建一个iOS Project
2. 在该工程下创建libs文件目录
3. 导入XMLib.framework包,将IMLib.framework拷贝至工程libs目录下
4. 将工程Build Settings中的Framework Search Paths指定到刚才的libs目录
5. Always Search user Paths设置为YES
6. 将Enable Bitcode设置为NO
7. 配置sdk运行所需权限

	```
	<!—IGIM_SDK  运行需要的权限  --/>
	```

8. Info.list增加 如下配置
 
>https配置
	
	<key>NSAppTransportSecurity</key>
    
    
>后台运行模式配置

	<key>UIBackgroundModes</key>

>存储设置

		<key>UIFileSharingEnabled</key>



### 调用IMClient接口，实现消息传递功能

>IMClient初始化


*application didFinishLaunchingWithOptions方法中，调用初始化接口*

	[[XMManager sharedInstance] initSdk:@”XXXXXX” withToken:@”XXXXXXXX-XXXX-XXXX_XXXX_XXXXXXXXXXXX”];//初始化，X符号在下载Demo时会被替换成开发者的专属字符串，请谨慎管理。


>推送绑定DeviceToken

*application didRegisterForRemoteNotificationsWithDeviceToken方法*
	[[XMManger sharedInstance] bindDeviceToken:deviceToken];	

>前后台切换处理

*applicationDidEnterBackground方法*
	
	[[XMManger sharedInstance] doBackground:nil succ:nil faild:nil];

*applicationDidEnterForeground方法*

	[[XMManger sharedInstance] doForeground];

>用户登录接口

	-(NSError *) loginWithParms:(XMLoginParams*)parm
	
	功能：		同步方式实现用户登录。

>用户下线接口

	-(void) logout:(XMSucc)succ fail(XMFail)fail
	
	功能：		登出

>构建消息

	-(instancetype)initWithConversation:(XMConversation*)conversation
	    
	功能：		构建消息

>发送消息
	
	-(int) sendMessage(XMMessage)msg succ:(XMSucc)succ fail(XMFail)fail
	
	功能：		发送消息

>获取当前登录的用户

	-(void) asyncGetContacts:(void (^)(NSArray*)successBlock failue:(void)(^)(NSError*)failureBlock)
	

	-（void）getLocalMessage:(int)count last:(XMMessage*)last succ:(XMGetMsgSucc)succ fail: (XMFail)fail
	

	-(NSArrary*)getAllConversation
	功能：		获取所有会话

	-(XMConversation*)getConversation:(XMConversationType)type receiver:(NSString*)receiver
	

	-(BOOL)deleteConversation: (XMConversation*)conversation deleteMessage:(BOOL)enable
	

	- (XMGroup)createGroupWithSubject:(NSString*)aSubject
          


>至此，游戏的聊天功能已经完全实现，能够发送与接收语音或者文本消息。