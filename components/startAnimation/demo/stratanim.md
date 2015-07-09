# 基本

- order: 1

页面进场动画效果示例。

---
````jsx
//new antd.startAnimation("要执行的dom","动画的数据","延时");
new antd.startAnimation(".demo-startAnim",[
[{style:"x-right"},[{style:"transform: scale(0)"},{style:"x-right"},{style:"x-right"},{style:"x-right"},{style:"x-right"}]],
[
{style:"x-right"},
[[{style:"x-right"},{style:"transform: scale(0)"},{style:"x-right"}]],
{style:"x-right"},
[[{style:"y-bottom"},{style:"y-bottom"}]]
],
{style:"y-bottom"}
])
$(function (){
$("#overBtn").bind("click",function (e){
new antd.startAnimation(".demo-startAnim",[
[{style:"x-right"},[{style:"x-right"},{style:"x-right"},{style:"x-right"},{style:"x-right"},{style:"x-right"}]],
[
{style:"x-right"},
[[{style:"x-right"},{style:"x-right"},{style:"x-right"}]],
{style:"x-right"},
[[{style:"y-bottom"},{style:"y-bottom"}]]
],
{style:"y-bottom"}
])
});
$("#listBtn").bind("click",function (e){
new antd.startAnimation(".demo-list",[
[{style:"y-bottom"},[{style:"y-bottom"},{style:"y-bottom"},{style:"y-bottom"},{style:"y-bottom"},{style:"y-bottom"}]],
[{style:"y-bottom",delay:0.2,delayRewrite:true},[{style:"y-bottom"},{style:"y-bottom"},{style:"y-bottom"},{style:"y-bottom"},{style:"y-bottom"}]],
])
});
})
````
<div style="text-align: center;margin: 20px auto;">
<button class="ant-btn ant-btn-primary" id="overBtn">
  刷新页面进场播放
</button>
<button class="ant-btn ant-btn-primary" id="listBtn">
  刷新局部进场播放
</button>
</div>
<div class="demo-startAnim">
      <div class="demo-header">
          <div class="logo"><img width="30" src="https://t.alipayobjects.com/images/rmsweb/T1B9hfXcdvXXXXXXXX.svg"> logo</div>
          <ul>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
              <li></li>
          </ul>
      </div>
      <div class="demo-content">
          <div class="demo-title">我是标题</div>
          <div class="demo-kp">
              <ul>
                  <li></li>
                  <li></li>
                  <li></li>
              </ul>
          </div>
          <div class="demo-title">我是标题</div>
          <div class="demo-listBox">
              <ul>
                  <li>
                      <div class="demo-list">
                          <div class="title"></div>
                          <ul>
                              <li></li>
                              <li></li>
                              <li></li>
                              <li></li>
                              <li></li>
                          </ul>
                      </div>
                  </li>
                  <li>
                      <div class="demo-list">
                          <div class="title"></div>
                          <ul>
                              <li></li>
                              <li></li>
                              <li></li>
                              <li></li>
                              <li></li>
                          </ul>
                      </div>
                  </li>
              </ul>
          </div>
      </div>
      <div class="demo-footer" style="width:100%;display: table;float:left"></div>
  </div>

<style>

.code-boxes-col-2-1 {
  width:100%;
}
/*****/
.demo-startAnim{
    width: 400px;
    text-align: center;
    overflow: hidden;
    margin: 20px auto;
}
.demo-startAnim .demo-header{
    width: 100%;
    background: #ebedee;
    height: 30px;
}
.demo-startAnim .logo{
    float: left;
    margin: 7px auto 0 10px;
}
.demo-startAnim .demo-header ul{
    float: right;
    margin-right: 10px;
}
.demo-startAnim .demo-header ul li{
    width: 30px;
    height: 30px;
    float: left;
    background: #e4e4e4;
    margin-left: 2px;
}

.demo-startAnim .demo-content{
    width: 80%;
    margin:10px auto;
}
.demo-startAnim .demo-content .demo-kp{
    margin: 10px auto;
}
.demo-startAnim .demo-content .demo-kp ul li{
    display: inline-block;
    width: 30%;
    height: 30px;
    background: #cacaca;
    color: #ebeded;
    text-align: left;
    padding: 10px;
    margin-right: calc(5% - 4px);;
}
.demo-startAnim .demo-content .demo-kp ul li:last-child{
    margin-right: 0%;
}
.demo-startAnim .demo-header ul li:before,
.demo-startAnim .demo-content .demo-kp ul li:before,
.demo-startAnim .demo-content .demo-kp ul li:after,
.demo-startAnim .demo-content .demo-listBox .demo-list .title:before,
.demo-startAnim .demo-content .demo-listBox .demo-list ul li:before,
.demo-startAnim .demo-content .demo-listBox .demo-list ul li:after,
.demo-startAnim .demo-footer:before,.demo-startAnim .demo-footer:after{
    display: block;
    content: "";
}
.demo-startAnim .demo-content .demo-kp ul li:after{
    width: 60%;
    height: 5px;
    background: #ebeded;
    float: left;
    margin-top: 2px;
}
.demo-startAnim .demo-content .demo-kp ul li:before{
    background: #ebeded;
    float: left;
    width: 10px;
    height: 10px;
    margin-right: 10%;
}
.demo-startAnim .demo-header ul li:before{
    margin: 10px auto;
    width:10px;
    height: 10px;
    background: #ebeded;
}
.demo-startAnim .demo-content .demo-title{
    background: #a4a4a4;
    width: 40%;
    height: 20px;
    line-height: 20px;
    color: #ebeded;

}
.demo-startAnim .demo-content .demo-listBox{
    margin-top: 10px;
}
.demo-startAnim .demo-content .demo-listBox>ul>li{
    float: left;
    width: 47.5%;
    overflow: hidden;

}
.demo-startAnim .demo-content .demo-listBox>ul>li:last-child{
    margin-left: 5%;
}
.demo-startAnim .demo-content .demo-listBox .demo-list .title{
    height: 25px;
    background: #cacaca;
    overflow: hidden;
}
.demo-startAnim .demo-content .demo-listBox .demo-list .title:before{
    width: 50%;
    height: 5px;
    background:#ebeded;
    margin: 10px auto;
}
.demo-startAnim .demo-content .demo-listBox .demo-list ul li{
    height: 20px;
    background: #ebeded;
    border-bottom: 1px solid #cacaca;
    overflow: hidden;
    padding: 5px 15px;
}
.demo-startAnim .demo-content .demo-listBox .demo-list ul li:before{
    width: 20px;
    height: 10px;
    background: #cacaca;
    float: left;
}
.demo-startAnim .demo-content .demo-listBox .demo-list ul li:after{
    width: 60%;
    height: 5px;
    background: #cacaca;
    float: left;
    margin-left: 10px;
    margin-top: 2px;
}
.demo-startAnim .demo-footer{
    margin-top: 10px;
    background: #cacaca;
    height: 30px;
    float: left;
    width: 100%;
}
.demo-startAnim .demo-footer:before{
    width: 60%;
    height: 5px;
    background: #ededed;
    margin: 5px auto 0;
}
.demo-startAnim .demo-footer:after{
    width: 30%;
    height: 5px;
    background: #ededed;
    margin:5px auto;
}
</style>
