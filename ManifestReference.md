# Manifest Reference

## Manifest.json文件示例
```json
{
  "name": "Comm100Bot", //App的名字
  "author": {
    "name": "Comm100", //开发者的名字
    "email": "developer@comm100.com",  //开发者的邮箱
    "url": "https://www.comm100.com/bot" //开发者的主页地址或App的主页地址
  },
  "private": false,  //该APP是否是私有APP的标志
  "version": "1.0",
  "frameworkVersion": "1.0",
  "location": {
    "controlPanel_topBar": "/manage/index.html",
    "agentConsole_chatSidebar": "/sidebar/index.html",
    "controlPanel_navigationMenu":{
       "url":"/navigationBar/secondMenu.html", 
       "product": "livechat"                   
     }
  },
  "visitor": {
    "customCSS": [
      "/assets/visitor_custom_css.css"
    ],
    "customJS": [
      "/assets/visitor_custom_js.js"
    ]
  },
  "webhook":[{
    "name": "app.installed",            //webhook名称 
    "url": "******/logicalProcess"      //webhook请求地址
  }],
  "objects":[{
    "name": "Bot",
    "property":[{
      "name": "BotId",
      "type": "string"
    },{
      "name": "BotName",
      "type": "string"
    }]
  }],
  "bot": {
    "endpoint": {
      "list": {     //机器人列表Endpoint配置
        "method": "POST",
        "url": "https://api.chatbot.com/comm100/{siteId}/bots", // {siteId}为宏, Comm100在处理这个地址时会替换掉这个参数
        "headers": {
          "Authorization": "Bearer {app.metadata.token}", // {app.metadata.token} 为后台安装这个app时设置的元数据, 
          "x-api-version": "v2",
        }
      },
      "message": {   //机器人列表回答问题Endpoint配置
        "method": "POST", //必须为post
        "url": "https://api.chatbot.com/comm100/{siteId}/bots/{botId}/message", // {botId}为宏参，表示使用的bot Id, 用户可以选择设置或者不设置, body中的内容也会有botId的属性
        "headers": {
          "Authorization": "Bearer {app.metadata.token}", // {app.metadata.token} 为后台安装这个app时设置的元数据, 
          "x-api-version": "v2",
        }
      }
    }
  }
}
```
## Manifest属性
  - [name](#name)
  - [author](#author)
  - [private](#private)
  - [version](#version)
  - [frameworkVersion](#frameworkversion)
  - [location](#location)
  - [visitor](#visitor)
  - [webhook](#webhook)
  - [objects](#objects)
  - [bot](#bot)

## Name
  指定App的名字，在Manifest中定义的App出现的位置也会以这个名字命名，如应用程序管理、产品菜单、tab标题等位置。必须指定。

## author 
  指定App开发者的信息，为一个包含`name`、`email`、`url`属性的json对象。必须指定。
   - `name` 指定当前App的开发者的名字。
   - `email` 指定当前App的开发者的邮箱。
   - `url` 指定当前App的开发者的主页地址或App的主页地址。

   ```json
   "author": {
      "name": "Comm100",                    
      "email": "developer@comm100.com",     
      "url": "https://www.comm100.com/bot"  
   }
   ```

## private
  指定当前App是否是私有的，默认值为`true`。私有的App只能被App开发者安装和使用，需要提交到Marketplace的App该属性必须设置为`false`。

   ```json
   "private": false
   ```

## version
  指定当前App的版本。该版本信息出现在Marketplace的App详细信息中。

   ```json
   "version": "1.0"
   ```

## frameworkVersion
  指定当前App开发过程中使用Comm100的框架版本。必须指定。

   ```json
   "frameworkVersion": "1.0"
   ```

## location
  指定当前App安装以后App出现在Comm100的产品界面中的位置。必须指定。  

  - `controlPanel_topBar` 指定将App安装在Control Panel的右上方产品级菜单区域
  - `controlPanel_navigationMenu` 指定将App安装在Control Panel的对应产品的左侧功能级菜单区域
  - `controlPanel_background` 指定App在Control panel启动后需要初始化的信息  
  - `agentConsole_topBar` 指定将App安装在Agent Console的头部全局菜单区域
  - `agentConsole_navigationBar` 指定将App安装在Agent Console的左侧列表菜单区域
  - `agentConsole_chatHeadTag` 指定将App安装在Agent Console的聊天窗口的用户Tag区域
  - `agentConsole_chatSideBar` 指定将App安装在Agent Console的聊天窗口的右侧区域
  - `agentConsole_chatToolBar` 指定将App安装在Agent Console的聊天窗口的工具栏区域
  - `agentConsole_background` 指定App在Agent Console启动后需要初始化的信息  


  每个位置可指定下面的`autoHide`、`autoLoad`、`url`属性，而`product`和`parent`属性为`controlPanel_navigationMenu`所特有，其他位置无效。
  + `autoHide` 指定当前App在系统启动后是否默认隐藏。默认值为`false`。
  + `autoLoad` 指定当前App在系统启动以后是否自动加载。默认是为`false`。
  + `url` 指定当前位置下的iframe中显示的页面地址。必须指定。
  + `product` 指定当前App安装后应用在哪个产品的功能菜单中，仅在controlPanel_navigationMenu中有效。目前提供的`product`为`livechat`，controlPanel__navigationMenu中必须指定。
  + `parent` 指定当前App安装后应用在菜单中那个父亲节点下，仅在controlPanel_navigationMenu中有效。如果为一级菜单，则不需要设置`parent`节点，若需要设置为二级菜单，则需要设置作为下面那个菜单的子菜单。安装后默认作为所在菜单的最后一个子菜单。
    - `dashboard` 指定App安装后设置到Livechat产品的`dashboard`目录的最后一个子菜单
    - `getOnline` 指定App安装后设置到Livechat产品的`Get Online & Chat`目录的最后一个子菜单
    - `installation` 指定App安装后设置到Livechat产品的`Installation`目录的最后一个子菜单
    - `campaign` 指定App安装后设置到Livechat产品的`Campaign`目录的最后一个子菜单
    - `settings` 指定App安装后设置到Livechat产品的`Settings`目录的最后一个子菜单
    - `chatbot` 指定App安装后设置到Livechat产品的`Chatbot Question Base`目录的最后一个子菜单
    - `security` 指定App安装后设置到Livechat产品的`Security`目录的最后一个子菜单
    - `history` 指定App安装后设置到Livechat产品的`History`目录的最后一个子菜单
    - `reports` 指定App安装后设置到Livechat产品的`Reports`目录的最后一个子菜单
    - `social` 指定App安装后设置到Livechat产品的`Social Media`目录的最后一个子菜单
    - `integrations` 指定App安装后设置到Livechat产品的`Integrations & API`目录的最后一个子菜单
    - `maximumOn` 指定App安装后设置到Livechat产品的`MaximumOn`目录的最后一个子菜单
    
   ```json
   "location": {
     "agentConsole_chatSideBar": {
       "autoHide": false,
       "autoLoad": false,
       "url": "/sidebar/index.html"
     }
   }
   ```
   
   也可以只指定位置的url，其他都为默认值。如：

   ```json
   "location": {
     "agentconsole_chatSidebar": "/sidebar/index.html"
   }
   ```

   可以指定多个位置

   ```json
   "location": {
     "agentConsole_topbar": "/topbar/index.html",
     "agentConsole_background": "/assets/inital.html",
     "controlPanel_topBar": "/manage/index.html",         //指定产品级菜单
     "controlPanel_navigationMenu":{
       "url":"/navigationBar/secondMenu.html", //指定菜单的页面
       "product": "livechat",                  //指定菜单所在的产品为livechat
       "parent": "integrations"                //指定菜单作为Integrations & API菜单下面的子菜单
     }
   }
   ```

## visitor
  指定需要在访客端需要引入的对象，包括自定义CSS和JavaScript。安装APP以后, 前端在加载完系统的访客端对象的时候会加载该区域设置的自定义对象。
   - `customCSS` - 指定当前App在访客端需要引用的自定义CSS。
   - `customJS` - 指定当前App在访客端需要引用的自定义JavaScript。  


   ```json
   "visitor": {
     "customCSS": [   
       "/assets/visitor_custom_css.css"
     ],
     "customJS": [             
       "/assets/visitor_custom_js.js"
     ]
   }
   ```

## webhook
  指定在Comm100的特定webhook中的请求地址。安装APP以后, Comm100会在Manifest文件的`webhook`属性中配置的`name`的Webhook的位置请求`url`的地址。
   - `name` - 指定Webhook的名称，详情可参考[Webhook API](https://github.com/hgq719/docs/blob/master/WebhookApi.md)。
   - `url` - 指定当前的webhook中需要请求的地址。  


   ```json
   "webhook": [{
     "name": "app.installed",
     "url": "****/logicalProcess"
   }]
   ```

## objects
  指定将外部对象引入到Comm100中，通过配置外部对象属性与Comm100对象属性的映射关系来实现系统之间的数据流转。安装App以后，可以通过Comm100的API来设置和获取外部对象。
  - `name` - 指定需要引入的外部对象的对象名
  - `property` - 指定当前引入的外部对象的属性
  - `fieldName` - 指定外部对象的字段名称
  - `fieldType` - 指定外部对象的数据类型,类型包括:`int/float/string/date`

  ```json
  "objects":[{
      "name": "Bot",
      "property":[{
        "fieldName": "BotId",
        "fileType": "string"
      },{
        "fieldName": "BotName",
        "fileType": "string"
      }]
    }]
  ```

## bot
  指定需要将自己的Bot系统接入Comm100产品需要进行的配置，安装该Bot App以后，可配置使用当前Bot参与聊天。
  - `endpoint` 指定接入Bot需要配置的Endpoint节点。
  - `list` 指定接入Bot时获取Bot列表的接口信息
  - `message` 指定接入Bot时，Bot回答问题的接口信息
  - `method` 指定接入Bot时对应接口的请求方式：`POST/GET` ,目前Bot的两个接口全部为`POST`
  - `url` 指定接入Bot时调用的Bot的接口地址
  - `headers` 指定接入Bot调用Bot的接口的头部信息


   ```json
   "bot": {
     "endpoint": {
       "list": {     //机器人列表Endpoint配置
         "method": "POST",
         "url": "https://api.chatbot.com/comm100/{siteId}/bots", // {siteId}为宏, Comm100在处理这个地址时会替换掉这个参数
         "headers": {
           "Authorization": "Bearer {app.metadata.token}", // {app.metadata.token} 为后台安装这个app时设置的元数据, 
           "x-api-version": "v2",
         }
       },
       "message": {   //机器人列表回答问题Endpoint配置
         "method": "POST", //必须为post
         "url": "https://api.chatbot.com/comm100/{siteId}/bots/{botId}/message", // {botId}为宏参，表示使用的bot Id, 用户可以选择设置或者不设置, body中的内容也会有botId的属性
         "headers": {
           "Authorization": "Bearer {app.metadata.token}", // {app.metadata.token} 为后台安装这个app时设置的元数据, 
           "x-api-version": "v2",
         }
       }
     }
   }
   ```
