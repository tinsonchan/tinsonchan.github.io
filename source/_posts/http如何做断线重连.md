---
title: http如何做断线重连
comments: true
date: 2018-05-31 22:15:26
categories:
- 技术
tags:
- egret
---
今天突然收到一个需求，就是做弱联网的断线重连。

拜托，我们游戏是http通信的呀，这如何是好。但是我们技术的童鞋嘛，总会有方法的是吧！

脑子里一下子就有了方案，就是做一个队列，然后把每次发送的post数据存起来，然后在收到回复的时候，在把队列里对应的删除掉，然后其他的，设置了一个三秒的超时记录，然后每秒去检查一次，如果发现这个超时了，就重发一次。

<!--more-->

首先做一个唯一id的存储，主要是方便自己去遍历用的

     	private static _sendID : number  = 0;
     	private static getSendID() : number{
        	return HttpUtil._sendID++;
     	}

然后就是队列数据的添加和删除了

    	private static _sendList : Dictionary<any> = new Dictionary<any>();
        private static addSendData(id : number, url : string , data : any) : boolean{
            let item = HttpUtil._sendList.get(id);
            let now = new Date();
            let time = now.getTime();
            if(item!= null){
                console.log("队列包含id为",id,"的数据了");
                return false;
            }
             
            HttpUtil._sendList.add(id, {url:url, json: data, time:time, isRemove: false, countTimes: 1});
            return true;
        }

        private static removeSendData(id: number) : boolean{
            let data = HttpUtil._sendList.get(id);
            if(data == null){
                console.log("队列没有包含id为",id,"的数据");
                return false;
            }
            data.isRemove = true;
            return true;
        }

    
基本上到这里就已经完成了大部分工作，后面就是做一个队列检查的工作，我基本上就是每一秒检查一次，如果发现超时的就会重发

     	private static checkList(){
             let now = new Date();
             let time = now.getTime();
             let keys = HttpUtil._sendList.keys;
             for(var i = 0; i < keys.length; i++){
                 let data = HttpUtil._sendList.get(keys[i]);
                 if(data != null){
                     //需要删除的
                     if(data.isRemove){
                         HttpUtil._sendList.remove(keys[i]);
                         continue;
                     }

                     //超时需要重发的,3s算超时
                     if(time - data.time > 3000){
                         console.log("需要重发的数据",data)
                         HttpUtil.getInstance().sendPostRequest(data.url, data.json);
                         HttpUtil._sendList.remove(keys[i]);
                     }
                 }
             }
         }

这就是整个重发机制的实现，整个实现就花了半个小时，主要是在做测试的地方耽误了一点点时间，也许这不完美，但是可以简单的实现策划的一个需求，记录一下，给后面的自己备份一下，方便查看，后面如果有好的方案，也对比一下。

----------
追求卓越 成功就会在不经意间追上你
