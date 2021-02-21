### Under Construction

本项目仍在建设中...

预计两天内上传成品

### Qletram-UI 简介

高效的Telegram-Like聊天UI框架，实测在20个会话，每个会话1000条消息下仍能流畅渲染。

目前并没有采用 Vue,React等前端框架，采用ES6原生的templete string渲染消息，采用mdui提供的轻量级工具库对DOM进行操作。

![主界面UI](/img/image-20210221095938701.png)

![抽屉栏](/img/image-20210221100635478.png)

![templete string 你可以方便地自定义其内容和布局](/img/image-20210221100434321.png)

### 如何使用？

下载项目到本地，之后在index.html中以 module 方式引入你自己的js

同时在你的js中以es6的方式引入模块

```javascript
import Qltegram from "./qltegram.js"
//如果需要多语言支持，则可以引入语言包模块
import trans from "./trans.js"

```

```javascript
Qlegram.updateData(yourdata);
//初始化加载聊天数据，其格式可以参照sampleData.json
Qlegram.renderChatList();
//渲染会话列表

//API
Qlegram.pushMsg(contactID,msgdata);
//增加一条消息，contactID为联系人ID,必须和初始化时给定的ID一致,msgdata格式参照sampleData.json里每个chat的msgItems的子项
//如果是当前会话则立即渲染，否则推送到CacheMsg中，等待渲染
//注意，假如该消息的发送者不在初始化给定的聊天数据的列表里（即临时会话），UI会自动调用获取联系人数据接口，并等待获取完毕后再执行渲染
Qlegram.setChatStaus(contactID,Status);
//设置对话的状态，contactID为联系人ID。必须和初始化时给定的ID一致
//Status为以下值的组合 2-置顶 4-收纳 16-静音 32-隐藏
Qlegram.saveChatsData()
//返回现在的chatdata,可以保存到本地，下次加载时预载
Qlegram.pushMsgArray(array)
//添加一个数组的消息，每个子项为{contactID:...,msgdata:...},规则同pushMsg()
//可用于离线消息
Qlegram.onGetContactInfo(promise)
//绑定一个回调函数，当框架要获取联系人信息的时候调用，注意，为了实现异步获取，该函数必须返回一个es6中新增的promise对象
//demo:
function getContactInfo(contactID){
    return new Promise(resolve, reject){
        let xhr= new XMLhttpRequest();
        xhr.open('GET',url+"?contactID="+contactID,false);
        //通过AJAX等方式获取信息
        xhr.onreadystatechange=function(){
            if(xhr.readyState==4){
                if(xhr.status==200 || xhr.status==304){
                    resolve(xhr.responseText);//回调给Promise
                }
            }
        }
        xhr.send();
    }
}
//绑定的时候如下绑定
Qlegrame.onGetContactInfo(getContactInfo);
//demo end
Qlegram.onGetGroupInfo(promise)
//绑定一个回调函数，当框架要获取群组信息的时候调用，注意，为了实现异步获取，该函数必须返回一个es6中新增的promise对象
Qlegrame.pushNoticeMsg(msgdata)
//增加一个通知消息，比如添加好友，添加群申请，详见demo

```

