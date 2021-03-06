# Introduction
  Comm100AgentConsoleAPI Client可以让开发者的App与Comm100的产品进行交互。开发者可以使用client来监听事件、设置/获取属性、执行特定方法。如：

  ```javascript
  var client = Comm100AgentConsoleAPI.init();
  client.get("currentAgent.name").then(function(data){
      console.log(data); //{"currentAgent.name":"kim"}
  });
  ```

## Comm100AgentConsole Object
  Agent Console中调用API的对象。

  - Actions
    + init -返回一个[client](#client-object)

  ```javascript
    Comm100AgentConsoleAPI.onReady = function () {
      // when this callback is triggered,
      // all APIs are ready to use
      var client = Comm100AgentConsoleAPI.init();
    }
  ```

## Client Object
Methods
  - [context](#client-context)
  - [get](#client-get)
  - [instance](#client-instance)
  - [do](#client-do)
  - [on](#client-on)
  - [off](#client-off)
  - [set](#client-set)
  - [trigger](#client-trigger)


### Client Context
  `client.context()` -请求当前对象的上下文信息，包括当前实例的id、位置等信息，返回一个`promise`对象。

  ```javascript
    var client = Comm100AgentConsoleAPI.init();
    client.context().then(function(context){
        console.log(context);
        /*
            {
                "instance_id":"013B7322-6895-F01E-5EAE-F4DF50016AF5",
                "location":"agentConsole_topBar",
                .....
            }
        */
    });
  ```

### Client Get
  `client.get(parameters)` -获取当前参数对应的值，返回一个`promise`对象。

  ```javascript
    var client = Comm100AgentConsoleAPI.init();
    client.get("currentChat.visitor").then(function(data){
        console.log(data);
        /*
            {
                "currentChat.visitor": {
                    ......
                }
            }
        */
    });
  ```

### Client Instance
  `client.instance(instanceId)` -App的每一个位置都是一个单独的实例，可以通过该方法来初始化App的一个特定位置的实例，用在实例间信息交互的时候。

  ```javascript
    var client = Comm100AgentConsoleAPI.init();
    var otherClient = Comm100AgentConsoleAPI.instance("013B7322-6895-F01E-5EAE-F4DF50016AF5");
  ```

### Client Do
  `client.do(name [,...args])` -调用`name`标识的操作，根据要求传递参数，返回一个`promise`对象。
  
  ```javascript
    var client = Comm100AgentConsoleAPI.init();
    client.do("preload").then(function(){
        //handler code
    });
  ```

### Client On
  `client.on(name, handler)` -在当前实例指定的`name`事件时，执行handler事件。

  ```javascript
  var client = Comm100AgentConsoleAPI.init();

  client.on("app.loaded",function(){
      //handler code
  });
  ```

  也可以通过`on`给当前实例添加一个`name`事件。

  ```javascript
  var client = Comm100AgentConsoleAPI.init();

  client.on("comming_call",function(){
      //handler code
  });
  ```

### Client Off
  `client.off(name, handler)` -给当前实例移除一个`name`事件。

  ```javascript
  var client = Comm100AgentConsoleAPI.init();

  client.off("comming_call",function(){
      //handler code
  });
  ```

### Client Set
  `client.set(key, value)` -设置当前`key`对应的属性的值为`value`，返回一个`promise`对象。

  ```javascript
  var client = Comm100AgentConsoleAPI.init();

  client.set("currentChat.email",email).then(function(){
      //handler code
  });
  ```

### Client Trigger
  `client.trigger(name, [data])` -触发指定的`name`的事件，`data`为该事件的参数

  ```javascript
    var client = Comm100AgentConsoleAPI.init();

    client.on("comming_call",function(){
        //handler code
    });

    client.trigger("comming_call");
  ```

## Core Apps API
  Comm100在App开发框架中提供了下面一些核心的事件、核心和行为供开发者在开发App的时候使用。
  - [Core Events](#core-events)
  - [Core Properties](#core-properties)
  - [core Actions](#core-actions)

### Core Events
  - [app.loaded](#app-loaded)
  - [app.activated](#app-activated)
  - [app.deactivated](#app-deactivated)
  - [instance.created](#instance-created)

#### App Loaded
  当App加载的时候触发的事件。

  ```javascript
  client.on("app.loaded",function(){
      //hanler code
  });
  ```

### App Activated
  当App被激活以后触发的事件。

  ```javascript
  client.on("app.activated",function(){
      //hanler code
  });
  ```
  
### App Deactivated
  当App从激活状态变成非激活状态以后触发的事件。

  ```javascript
  client.on("app.deactivated",function(){
      //hanler code
  });
  ```

### Instance Created
  当一个App的实例被创建的时候，其他的实例将收到`instance.created`事件通知。

  ```javascript
  client.on("insance.created",function(){
      //hanler code
  });
  ```

### Core Properties
  - [instances](#instances-property)

#### Instances Property
  表示App的所有正在运行的实例，每一个iframe都是一个实例，返回一个`promise`对象。同一个App在Manifest中定义了多个位置，则这个App就存在多个App Instance。

  get
  
  ```javascript
    client.get('instances').then(function(instancesData){
        console.log(instancesData);
        /*
        {
            "instances":[{
                "instance_id":"013B7322-6895-F01E-5EAE-F4DF50016AF5",
                "location":"agentConsole_topBar",
                ....
            }]
        }
         */
    });
  ```

### Core Actions
  - [resize](#resize-action)
  - [instances.create](#instance-create-action)

#### Resize Action
  重新设置App所在的iframe的页面大小，大小规格中的`width`和`height`可以是`px`、`%`或`vw/vh`。

  ```javascript
    client.do("resize",{
        width: "300px",
        height:"200px"
    });
  ```

#### Instances Create Action
  创建一个App的实例，实例属性包含`location`、`url`两个属性，目前`location`只支持`modal`，用来创建一个模态窗口的实例。

  ```javascript
    client.do("instances.create",{
        location: "modal",
        url:"****.html"
    });
  ```

## Agent Console Location
  Comm100在Agent Console端提供了可用的事件、对象、属性及操作，开发者可以使用这些来开发自己的App。每一个App实例都存在于Agent Console的某一个位置接口的iframe中，这些位置都定义了：
  - 所有的位置都共享了一些事件、属性及操作，详情参考[All Locations](#all-locations)
  - 每个位置有自己独特的事件、属性及操作
  

  下表中列出了Agent Console端所有可用的位置：
  - [agentConsole topBar](#agent-console-topbar) -Agent Console的头部全局菜单区域。
  - [agentConsole navigationBar](#agent-console-navigations) -Agent Console的左侧列表菜单区域。
  - [agentConsole chatToolBar](#agent-console-chattoolbar) -Agent Console的聊天页面中间的toolBar区域。
  - [agentConsole chatTabBar](#agent-console-chattabbar) -Agent Console的聊天窗口的右侧Tab区域。
  - [agentConsole modal](#agent-console-modal) -Agent Console中配置一个模态窗口的形式的页面。
  - [agentConsole background](#agent-console-background) -Agent Console中以后台形式(不显示UI)启动的页面。


  开发者可以在Manifest文件中定义App的所在位置，具体参考[Setting The Location](https://github.com/hgq719/docs/blob/master/open-platform.md#setting-the-location)。  
  如果开发者指定`agentconsole_topBar`，App只展示一个图标，点击图标可以打开这个App。

## All Locations
  下面的这些对象、属性及操作在Agent Console中的所有位置都是可用的。

  Objects
  - [agent object](#agent-object)


  Additional actions
  - [notify](#notify-action)
  - [routeTo](#routeto-action)

### Agent Object
  `currentAgent` -当前登录的agent对象

  get
  ```javascript
  client.get("currentAgent").then(function(currentAgent){
      console.log(currentAgent);
      /*
      {
        "id": 1,    // number
        "name": '', // string
        "email": '',// string
        "status": '', // string, online/away
        "apikey": '', // string, 
        "chats": 3,   // number, ongoing chats
        "isAdmin": false, // boolean
      }
      */
  });
  ```

  Properties
  - `agent.id` -当前agent的Id
  - `agent.name` -当前agent的名称
  - `agent.email` -当前agent的邮箱
  - `agent.status` -当前agent的状态
  - `agent.apikey` -当前agent的apikey
  - `agent.chats` -当前agent正在进行的聊天
  - `agent.isAdmin` -当前agent是否是管理员


### Additional Actions  

#### Notify Action
 发送一个通知

  Notification Properties
+ `message` -通知的消息主体内容
+ `duration` -通知的持续显示时间，默认3秒
+ `sticky`-通知是否一直显示，直到用户点击关闭才消息的标志。`true/false`，默认为`false`，即消息不会一直显示。
+ `type`-通知类型
  * `notice` -普通通知
  * `warning` -警告
  * `error` -错误

 ```javascript
  // body struct
  const notification =  {
    message: "there is a phone call for you!",
    duration: 4,
    sticky: false, 
    type: "notice" 
  }

  client.do("notify",notification);
```

#### RouteTo Action
  直接定位到特定的Tab

  Arguments
+ `tabType` -tab的类型，默认值为`navigationBar`
+ `tabName` -打开tab的名称，目前包括`Visitors`、`MyChats`、`Agents`和自定义的tab。
+ `appSection`-可选参数，目前在`tabName=Mychats`时有效，可设置为访客名称定位到对应访客的聊天
。

 ```javascript
  client.do("routeTo","navigationBar","MyChats","kim");
```

## Agent Console TopBar
  指定将App安装在Agent Console的头部全局菜单区域，呈现的方式是一个icon图标，点击该图标即可打开App。当App第一次被打开的时候，Comm100会在Dom中添加一个`Pane`容器，容器中展示的即为用户配置的地址的页面内容。

  ```json
  "location": {
    "agentConsole_topBar": "/assets/topBar.html"
  }
  ```

#### TopBar Actions
  在Control Panel的TopBar中，app可执行下面的动作：
  - [popup](#popup-action)
  - [preloadPane](#preloadpane-action)

##### Popup Action
  开发者可通过`popup`的API来显示、隐藏topBar App窗口。

 popup Properties
+ `visible`-弹出窗口的显示状态: show/hide 默认show

 ```javascript
  client.do("popup","show");
 ```

##### PreloadPane Action
  在TopBar App还没有被打开过的情况下，`Pane`容器还没有被加载到Dom中来，开发者可通过`preloadPane`的API来预加载`Pane`。

  ```javascript
    client.do("preloadPane");
  ```

#### TopBar Events
  在Control Panel的TopBar中，app可使用的下面的事件：
  - [pane.activated](#topbar-pane-activated)
  - [pane.deactivated](#topbar-pane-deactivated)

##### TopBar pane.activated
  可通过下面的API来设置在pane激活的时候需要做的事情。

  ```javascript
    client.on("pane.activated", function(){
      //handler code
    }
  ```

##### TopBar pane.deactivated
  可通过下面的API来设置在pane取消激活的时候需要做的事情。

  ```javascript
    client.on("pane.deactivated", function(){
      //handler code
    }
  ```

## Agent Console NavigationBar
  指定将App安装在Agent Console的左侧导航区域，呈现的方式是一个`tab`，点击该tab即可打开App。
  
  ```json
  "location": {
    "agentConsole_navigationBar": "/assets/navBar.html"
  }
  ```

## Agent Console ChatToolBar
  指定将App安装在Agent Console的聊天窗口中间的ToolBar中，呈现的方式是一个icon图标，点击该图标即可打开App。当App第一次被打开的时候，Comm100会在Dom中添加一个`Pane`容器，容器中展示的即为用户配置的地址的页面内容。

 ```json
  "location": {
    "agentConsole_chatToolBar": "/assets/toolBar.html"
  }
  ```

#### ToolBar Actions
  在Control Panel的ToolBar中，app可执行下面的动作：
  - [popup](#toolbar-popup-action)
  - [preloadPane](#toolbar-preloadpane-action)

##### ToolBar Popup Action
  开发者可通过`popup`的API来显示、隐藏ToolBar App窗口。

 popup Properties
+ `visible`-弹出窗口的显示状态: show/hide 默认show

 ```javascript
  client.do("popup","show");
 ```

##### ToolBar PreloadPane Action
  在ToolBar App还没有被打开过的情况下，`Pane`容易还没有被加载到Dom中来，开发者可通过`preloadPane`的API来预加载`Pane`。

  ```javascript
    client.do("preloadPane");
  ```

#### ToolBar Events
  在Control Panel的ToolBar中，app可使用的下面的事件：
  - [pane.activated](#toolbar-pane-activated)
  - [pane.deactivated](#toolbar-pane-deactivated)

##### ToolBar pane.activated
  可通过下面的API来设置在pane激活的时候需要做的事情。

  ```javascript
    client.on("pane.activated", function(){
      //handler code
    }
  ```

##### ToolBar pane.deactivated
  可通过下面的API来设置在pane取消激活的时候需要做的事情。

  ```javascript
    client.on("pane.deactivated", function(){
      //handler code
    }
  ```

## Agent Console ChatTabBar
  指定将App安装在Agent Console的聊天窗口的右侧TabBar中，呈现的方式是一个`tab`，点击该tab激活app，在tab的下方展示app的内容。

```json
  "location": {
    "agentConsole_chatTabBar": "/assets/chatTabBar.html"
  }
  ```

### ChatTabBar Objects
  在聊天区域的TabBar中，可用的对象如下：
  - [visitor](#visitor-object)
  - [chat](#chat-object)
  - [agent](#agent-object)

  ### Visitor Object
  `visitor` -当前聊天中的访客对象

  get
  ```javascript
  client.get("currentVisitor").then(function(visitor){
      console.log(visitor);
      /*
      {
        id: 1,    // number
        name: '', // string
        email: '',// string
        status: '', // string, waiting for chat/chatting/prechat/inviting/offline message/refused by agent/refused by visitor/chat ended/in site/out of site/transferring/system processing
        enterSiteTime: 1502934947,  // number, unix time
        referrer: '', // string, the referrer url
        landingPage: {
            title: '',  // string
            url: '',    // string
        }
        currentPage: {
            title: '',  // string
            url: '',    // string
        },
        currentPageStayTime: 20, // number, time of this visitor stay in current page, unit(second)
        numOfPages: 20, // number, the number of this visitor view pages
        location: {
            ip: '',   // string, ip address in dotted form
            city: '', // string
            state: '',// string
            country: '',  // string
        },
        device: {
            os: '', // string
            browser: '',  // string
            screenResolution: '', //string
            language: '', // string
            timezone: '', // string
        },
        history: {
            firstVisitorTime: 1502934947, // number, unix time
            visitTimes: 0,  // number, visit times of this visitor
            chatTimes: 0,   // number, chat times of this visitor
        }
        visitorSegmentations: [], // array<string>, names of segmentation
        customVariables: [
            {
            name: '', // string
            value: '',// string
            },
        ]
      }
      */
  });

  client.get("currentVisitor.email").then(function(email){
      console.log(email);
  ```

  set
  ```javascript
    client.set("currentVisitor.email","kim@comm100.com").then(function(){
        //handler code
    });
  ```

  Properties
  - `visitor.id` -当前聊天的visitor的Id
  - `visitor.name` -当前聊天的visitor的名称
  - `visitor.email` -当前聊天的visitor的邮箱
  - `visitor.status` -当前聊天的visitor的状态
  - `visitor.enterSiteTime` -当前聊天的visitor进入站点的事件
  - `visitor.referrer` -当前聊天的visitor的来源站点
  - `visitor.landingPage` -当前聊天的visitor访问当前站点的着陆页面对象
    + `title` -页面标题
    + `url` -页面地址
  - `visitor.currentPage` -当前聊天的visitor正在访问的页面对象
    + `title` -页面标题
    + `url` -页面地址
  - `visitor.currentPageStayTime` -当前聊天的visitor在当前页面的停留时间
  - `visitor.numOfPage` -当前聊天的visitor访问当前站点页面的数量
  - `visitor.location` -当前聊天的visitor的地址
    + `ip` -ip地址
    + `city` -城市
    + `state` -州、省
    + `country` -国家
  - `visitor.device` -当前聊天的visitor的设备信息
    + `os` -操作系统
    + `browser` -浏览器
    + `screenResolution` -屏幕分辨率
    + `language` -语音
    + `timezone` -时区
  - `visitor.history` -当前聊天的visitor的历史记录
    + `firstVisitorTime` -首次访问时间
    + `visitTimes` -访问次数
    + `chatTimes` -聊天次数
  - `visitor.visitorSegmentations` -当前聊天的visitor的细分数组
  - `visitor.customVariables` -当前聊天的visitor的自定义变量
    + `name` -变量名
    + `value` -变量值
  - `visitor.ssoFlag` -当前聊天的visitor的SSO标志

### Chat Object
  `currentChat` -当前聊天对象

  get
  ```javascript
    client.get("currentChat").then(function(currentChat){
      console.log(currentAgent);
      /*
      {
        id: 1,  // number
        visitorId: 1, // number
        status: ''  // string,  chatting/waiting/transferring/chat ended/audio chatting/video chatting/inviting
        name: '',     // string
        email: '',    // string 
        company: '',  // string
        phone: '',    // string
        productService: '', // string
        department: '',     // string, name of department
        fields: [
            {
            name: '',   // string
            value: '',  // string
            }
        ]
        rating: {
            score: 1,   // number, 0 - 5
            comment: '',  // string,
        }
        agents: [], // array<string>, agent's name
        requestPage: {
            title: '',  // string
            url: '',    // string
        },
        requestTime: 1502934947,  // number, unix time
        waitingTime: 15,  // number, how long the visitor waiting time, unit(second)
        duration: 180, //number, how long the chat duration, unit(second)、
        messages: [
            {
            sender: '',
            senderType: '', // string, system/agent/visitor
            time: 1502934947,
            content: '',  // string, html 
            }
        ]
      }
      */
    });

    client.get("currentChat.messages").then(function(messages){
      console.log(messages);
    }
  ```

Properties
  - `chat.id` -当前聊天的Id
  - `chat.visitorId` -当前聊天的访客id
  - `chat.name` -当前聊天的访客名称
  - `chat.email` -当前聊天的访客邮箱
  - `chat.status` -当前聊天的状态 `chatting/waiting/transferring/chat ended/audio chatting/video chatting/inviting`
  - `chat.company` -当前聊天的访客的公司
  - `chat.phone` -当前聊天的访客的电话
  - `chat.fields` -当前聊天的访客填入的字段信息
    + `name` -字段名
    + `value` -字段值
  - `chat.rating` -当前聊天的访客评分
    + `score` -分值
    + `comment` -备注
  - `chat.agents` -当前聊天的客服数组
  - `chat.requestPage` -当前聊天访客的初始请求页面
    + `title` -页面标题
    + `url` -页面地址
  - `chat.requestTime` -当前聊天的访客的请求时间
  - `chat.waitingTime` -当前聊天的访客的等待时间
  - `chat.duration` -当前聊天的持续时间
  - `chat.messages` -当前聊天的信息
    + `sender` -信息发送对象
    + `senderType` -信息发送类型， `system/agent/visitor`
    + `time` -信息发送时间
    + `content` -信息内容

### ChatTabBar Actions
  在ChatTabBar中，App中可使用下面的行为:
  - [Chat Acionts](#chattabbar-chat-actions)

#### ChatTabBar Chat Actions
  在ChatTabBar中，对于聊天对象app可用的行为如下：
  - `send` -在当前聊天中发送一条信息
  - `input` -在当前聊天的输入框中插入一条信息

```javascript
  client.do("currentChat.send","newMessage");
  client.do("currentChat.input","inputMessage");
```

### ChatTabBar Events
  在ChatTabBar中，对于聊天对象可用的事件如下：
  - [chats.chatStart](#chats-start)
  - [chats.chatEnd](#chats-end)
  - [Chats.selectChange](#chats-select-change)
  - [currentChat.submitWrapup](#chat-submit-Wrapup)
  - [currentChat.receiveVisitorMessage](#chat-receive-visitor-message)
  - [currentChat.visitor.status.change](#visitor-status-change)

#### Chats Start
  `chats.chatStart`事件发生在聊天开始的时候，开发者可以通过该事件指定需要在聊天开始的时候所做的事情，该事件的处理程序接收一个[Chat](#chat-object)对象。

  ```javascript
    client.on('chats.chatStart',function(chat){
        //handler code
    });
  ```

#### Chats End
  `chats.chatEnd`事件发生在聊天结束的时候，开发者可以通过该事件指定需要在聊天结束的时候所做的事情，该事件的处理程序接收一个[Chat](#chat-object)对象。

  ```javascript
    client.on('chats.chatEnd',function(chat){
        //handler code
    });
  ```
  
#### Chats Select Change
  `chats.selectChange`事件发生在聊天选择变更的时候，开发者可以通过该事件指定需要在聊天选择变更的时候所做的事情，该事件的处理程序接收一个[Chat](#chat-object)对象。

  ```javascript
    client.on('chats.selectChange',function(chat){
        //handler code
    });
  ``` 

#### Chat Submit Wrapup
  `currentChat.submitWrapup`事件发生在聊天提交Wrapup的时候，开发者可以通过该事件指定需要在聊天提交Wrapup的时候所做的事情，该事件的处理程序接收一个[Chat](#chat-object)对象。

  ```javascript
    client.on('currentChat.submitWrapup',function(chat){
        //handler code
    });
  ``` 

#### Chat Receive Visitor Message
  `currentChat.receiveVisitorMessage`事件发生在当前聊天接收到访客信息的时候，开发者可以通过该事件指定需要在当前聊天接收到访客信息的时候所做的事情，该事件的处理程序接收一个`message`字符串。

  ```javascript
    client.on('currentChat.receiveVisitorMessage',function(message){
        //handler code
    });
  ``` 

#### Visitor Status Change
  `currentChat.visitor.status.change`事件发生在当前聊天的访客状态发生变更的时候，开发者可以通过该事件指定需要在聊当前聊天的访客状态发生变更的时候所做的事情，该事件的处理程序接收一个[Visitor](#visitor-object)对象。

  ```javascript
    client.on('chats.chatStart',function(visitor){
        //handler code
    });
  ```

## Agent Console Modal
  开发者可通过[instance.create](#instance-create-action)指定其`location`参数为`agentConsole_modal`来创建一个模态窗口实例。

  ```javascript
    client.do("instance.create",{
        location: "agentConsole_modal",
        url:"*****.html"
    }).then(function(modalContext){
        //模态窗口直接显示出来
        var modalClient = client.instance(modalContext.instanceId);
        modalClient.on("modal.close",function(){
            //关闭当前模态窗口时需要进行的处理
        });
    });
  ```
### Modal Actions
  - [resize](#modal-resize-action)
  - [close](#modal-close-action)

#### Modal Resize Action
  开发者可通过此方法来修改模态窗口的大小。

  ```javascript
    client.do("resize",{
        width: "60vw",
        height: "50vh"
    });
  ```

#### Modal Close Action
  开发者可通过此方法来关闭当前模态窗口。

  ```javascript
    client.do("close",);
  ```

### Modal Events
  - [modal.close](modal-close-event)

#### Modal Close Event
  开发者可以在模态窗口关闭的时候指定需要进行的处理。

  ```javascript
    Client.on("modal.close",function(){
      //关闭当前模态窗口时需要进行的处理
    });
  ```

## Agent Console Background
  指定App在Agent Console中以后台形式(不显示UI)启动的页面，一般用于App初始化或持续运行的一些服务。

  ```json
  "location": {
    "agentConsole_background": "/assets/agentConsole_initial.html"
  }
  ```

### Agent Console Background Events
  除了框架本身的一些[核心事件](#core-events)外，在Agent Console的Background中还可以使用下面的事件来进行App的开发。
  - [chats.chatStart](#chats-start)
  - [chats.chatEnd](#chats-end)



