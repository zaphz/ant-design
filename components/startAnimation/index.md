# startAnimation

- category: CSS
- chinese: 开场动画JS
- order: 5

---

<script>
(function() {
/**
 * Created by jljsj on 15/7/3.
 */

var startAnim = function (node, data, delay, hideen) {
    this.nodeStr = node;
    this.doc = document;
    this.tweenData = data instanceof Array ? data : null;
    this.str = typeof data == "string" ? data : "x-right";
    this.delay = Number(delay) ? delay : 0;
    hideen = typeof hideen == "undefined" ? true : hideen;
    if (hideen) {
        this.doc.documentElement.style.opacity = 0;
        this.doc.documentElement.style.visibility = "hidden";
    }

    this.ready();
};
var a = startAnim.prototype = [];
a.error = function (msg) {
    throw new Error(msg);
};
a.ready = function () {
    var self = this;

    function detach() {
        if (self.doc.addEventListener) {
            self.doc.removeEventListener("DOMContentLoaded", completed, false);
            window.removeEventListener("load", completed, false);
        } else {
            self.doc.detachEvent("onreadystatechange", completed);
            window.detachEvent("onload", completed);
        }
    }

    function completed() {
        if (self.doc.addEventListener || event.type === "load" || self.doc.readyState === "complete") {
            detach();
            self.init();
        }
    }

    if (self.doc.readyState === "complete") {
        completed()
    } else if (self.doc.addEventListener) {
        self.doc.addEventListener("DOMContentLoaded", completed, false);
        window.addEventListener("load", completed, false);
    } else {
        self.doc.attachEvent("onreadystatechange", completed);
        window.attachEvent("onload", completed);
    }
};

a.init = function () {
    var self = this,
        regTag = (/^<(\w+)\s*\/?>(?:<\/\1>|)$/),
        rquickExpr = /^(?:#([\w-]+)|(\w+)|\.([\w-]+))$/;
    var htmlStyle = self.doc.documentElement;
    self.removeStyle(htmlStyle, "visibility:hidden;opacity:0");
    if (htmlStyle.style.length <= 0) {
        self.doc.documentElement.removeAttribute("style");
    }
    if (!self.nodeStr || regTag.test(self.nodeStr)) {
        return self.error("元素错误;");
    }
    if (typeof self.nodeStr == "string") {
        //var m=rquickExpr.exec(self.nodeStr);
        self.push.apply(self, self.doc.querySelectorAll(self.nodeStr))
    }
    if (!self.length) {
        return self.error("元素错误;");
    }
    for (var i = 0; i < self.length; i++) {
        var m = self[i];
        self.removeStyle(m, "visibility:hidden;opacity:0");
    }
    var mc = self.length == 1 ? self[0].children : self;
    self.forTweenData(mc, self.tweenData, function (mc, data) {
        if (!mc) {
            console.log("数据多余:" + JSON.stringify(data));
            return
        }
        mc.style.opacity = "0";
        mc.style.visibility = "visible";
        if (data) {
            if ((data.style || data.name) && !data.to) {
                self.addStyle(mc, self.animNameGroup(data.style || data.name))
            }
        } else {
            self.addStyle(mc, self.animNameGroup(self.str))
        }
    });
    var delay = self.delay || 10;
    setTimeout(function () {
        self.addTween();
    }, delay);
};

a.getTransform = function () {
    var style = "transform";
    if (!style in this.doc.documentElement.style) {
        var prefix = ['webkit', 'moz', 'ms', 'o'];
        for (var i in prefix) {
            style = "-" + prefix[i] + "-transform";
            if ("-" + prefix[i] + "-transform" in this.doc.documentElement.style) break;
        }
    }
    return style
};
a.getTransition = function () {
    var style = "transition";
    if (!style in this.doc.documentElement.style) {
        var prefix = ['webkit', 'moz', 'ms', 'o'];
        for (var i in prefix) {
            style = "-" + prefix[i] + "-transition";
            if ("-" + prefix[i] + "-transition" in this.doc.documentElement.style) break;
        }
    }
    return style
};
//遍历dom节点；
a.forTweenData = function (mc, data, callFunc) {
    if (!mc) {
        console.log("数据多余:" + JSON.stringify(data));
        return
    }
    var tm = mc.children || mc,
        i = 0;
    if (data && data.length) {
        for (i = 0; i < data.length; i++) {
            if (data[i].length) {
                this.forTweenData(tm[i], data[i], callFunc)
            } else {
                callFunc(tm[i], data[i])
            }
        }
    } else {
        for (i = 0; i < tm.length; i++) {
            if (mc.children) {
                callFunc(tm[i], null);
            } else {
                this.forTweenData(tm[i], null, callFunc);
            }
        }
    }
};
//添加样式类
a.addClass = function (m, value) {
    var _classname = m.className, s_k = " ";
    if (_classname.indexOf(value) < 0) {
        m.className += s_k + value;
    }
    m.className = m.className.trim()
};
//删除样式类
a.removeClass = function (m, value) {
    var rclass = /[\t\r\n\f]/g;
    var _classname = (" " + m.className + " ").replace(" " + rclass + " ", " ");
    while (_classname.indexOf(value) >= 0) {
        _classname = _classname.replace(value, " ");
    }
    m.className = _classname.trim();
    if (!m.className || m.className == "" || m.className == " ") {
        m.removeAttribute("class")
    }
};
//添加style
a.addStyle = function (mc, style) {
    var cArr = style.trim().split(";");
    var _style = mc.getAttribute("style");
    _style += style;
    mc.setAttribute("style", _style);
};
//删除style;
a.removeStyle = function (mc, style) {
    var cArr = style.trim().split(";");
    for (var i = 0; i < cArr.length; i++) {
        if (cArr[i] && cArr[i] !== "") {
            var sArr = cArr[i].split(":");
            var _style = mc.style.cssText.replace(sArr[0] + ":", "").replace(sArr[1] + ";", "");
            mc.setAttribute("style", _style)
        }
    }
};
a.addTween = function () {

    //查找tweenDataArr与dom下子级的匹配；
    var self = this, delay = 0, ease = "cubic-bezier(0.165, 0.84, 0.44, 1)", timer = 0.5;

    function whichAnimationEvent() {
        var animation = {
            'animation': 'animationend',
            'oAnimation': 'oanimationend',
            'MozAnimation': 'mozAnimationEnd',
            'WebkitAnimation': 'webkitAnimationEnd',
            'msAnimation': 'MSAnimationEnd'
        };
        for (var t in animation) {
            if (t in self.doc.documentElement.style) {
                return animation[t];
            }
        }
    }

    function whichTransitionEvent() {
        var transitions = {
            'transition': 'transitionend',
            'OTransition': 'oTransitionEnd',
            'MozTransition': 'transitionend',
            'WebkitTransition': 'webkitTransitionEnd'
        }

        for (var t in transitions) {
            if (t in self.doc.documentElement.style) {
                return transitions[t];
            }
        }
    }

    function setAnimEventEnd(mc, css) {
        var animationEvent = whichAnimationEvent();

        function _event(e) {
            if (self.doc.addEventListener) {
                mc.removeEventListener(animationEvent, _event)
            } else {
                window.detachEvent(animationEvent, _event);
            }
            self.removeClass(mc, css);
        }

        if (self.doc.addEventListener) {
            animationEvent && mc.addEventListener(animationEvent, _event);
        } else {
            animationEvent && mc.attachEvent(animationEvent, _event);
        }
    }

    function setTrnsitionEnd(mc) {
        var transitionEvent = whichTransitionEvent();

        function _event(e) {
            if (self.doc.addEventListener) {
                mc.removeEventListener(transitionEvent, _event)
            } else {
                window.detachEvent(transitionEvent, _event);
            }
            self.removeStyle(mc, "opactiy:1;visibility:visible");
            var s = mc.getAttribute("style").split(";");
            var i = 0, _style = "";
            while (i < s.length) {
                if (s[i] !== "") {
                    if (s[i].indexOf(mc.style[self.getTransition()]) >= 0) {
                        s[i] = "";

                    }
                    if (s[i] !== "") {
                        _style += s[i] + ";"
                    }
                }
                i++
            }
            mc.setAttribute("style", _style);
        }

        if (self.doc.addEventListener) {
            transitionEvent && mc.addEventListener(transitionEvent, _event);
        } else {
            transitionEvent && mc.attachEvent(transitionEvent, _event);
        }
    }

    var mc = self.length == 1 ? self[0].children : self;
    self.forTweenData(mc, self.tweenData, function (mc, data) {
        if (!mc) {
            console.log("数据多余:" + JSON.stringify(data));
            return
        }
        setTimeout(function () {
            var tweenStr = " " + timer + "s " + ease + " " + delay + "s";

            function fjStyle(mc, style) {
                var cArr = style.trim().split(";");
                for (var i = 0; i < cArr.length; i++) {
                    if (cArr[i] && cArr[i] !== "") {
                        var sArr = cArr[i].split(":");
                        mc.style[self.getTransition()] = sArr[0] + tweenStr;
                    }
                }
            }

            if (data) {
                var _delay = data.delay || delay,
                    _ease = data.ease || ease,
                    _timer = data.timer || timer;
                //重写延时
                delay = data.delayRewrite ? _delay : delay;
                tweenStr = " " + _timer + "s " + _ease + " " + _delay + "s";

                if (data.css) {
                    mc.style[self.getTransition()] = "none";
                    self.addClass(mc, data.css);
                    setAnimEventEnd(mc, data.css);
                } else if ((data.style || data.name) && !data.css) {
                    var _name = self.animNameGroup(data.style || data.name);
                    fjStyle(mc, _name);
                    if (data.to) {
                        self.addStyle(mc, _name)
                    } else {
                        self.removeStyle(mc, _name)
                    }
                }
            } else {
                fjStyle(mc, self.animNameGroup(self.str));
                self.removeStyle(mc, self.animNameGroup(self.str))
            }
            mc.style[self.getTransition()] = mc.style[self.getTransition()] ? mc.style[self.getTransition()] + "," + "opacity" + tweenStr : "opacity" + tweenStr;
            mc.style.opacity = 1;
            delay += 0.1;
            //console.log(mc.style[self.getTransition()])
            setTrnsitionEnd(mc)
        }, 10)
    });
};
a.animNameGroup = function (name) {
    var _style = "", self = this;
    switch (name) {
        case "x-left":
            _style = self.getTransform() + ":translateX(-30px)";
            break;
        case "x-right":
            _style = self.getTransform() + ":translateX(30px)";
            break;
        case "y-bottom":
            _style = self.getTransform() + ":translateY(30px)";
            break;
        case "y-top":
            _style = self.getTransform() + ":translateY(-30px)";
            break;
        case "scale":
            _style = self.getTransform() + ":scale(0)";
            break;
        case "scaleFrom":
            _style = self.getTransform() + ":scale(2)";
            break;
        case "scaleX":
            _style = self.getTransform() + ":scaleX(0)";
            break;
        case "scaleY":
            _style = self.getTransform() + ":scaleY(0)";
            break;
        default :
            _style = name;
            break;
    }
    return _style
};

antd.startAnimation = startAnim;gi
})();
</script>

##startAnimation的动画参数；

页面进场动画的类，通过ＣＳＳ来快速完成页面刷新后的动画进场或子块的动画进场；

### 用法
<pre><code>new antd.startAnimation(node,data,delay,hideen);
new antd.startAnimation(node,string)</code></pre>

### 参数说明

|参数             |类型    |详细                                                 |
|-----------------|-------|----------------------------------------------------|
|node             |string|要执行动画的dom（class,tag,id），不可为标签("<>");必要;  |
|data             |string / array|执行动画的参数，有array和string两种类型，下面详解；默认为null|
|delay            |number|整个区块的延时，默认为0                                |
|hideen           |boolean|在开始动画前隐藏掉html,默认为true;                     |

####data参数（string|array）;

支持css样式，内置name和style直接添加动画；

为string时，自动遍历node下的子节点来执行data样式；

为array时，树状形dom结构，以([],new Array)为一档标签；
如：

<pre><code>&lt;div class="a"&gt;
&lt;div class="b"&gt;&lt;/div&gt;
&lt;div class="c"&gt;&lt;/div&gt;
&lt;/div&gt;</code></pre>

node用的是".a",做b,c的动画，那data为：[]为最外层div;
<pre><code>[
{name:"x-left"},
{name:"x-left"}
]</code></pre>

如果元素为多个时：

<pre><code>&lt;div class="a"&gt;
&lt;ul&gt;
&lt;li&gt;&lt;span&gt;&lt;/span&gt;&lt;/li&gt;
&lt;li&gt;&lt;span&gt;&lt;/span&gt;&lt;/li&gt;
&lt;li&gt;&lt;span&gt;&lt;/span&gt;&lt;/li&gt;
&lt;/ul&gt;
&lt;/div&gt;</code></pre>

处理每个li里的span的动画时，data为:

<pre><code>[
[{name:"x-left"}],
[{name:"x-left"}],
[{name:"x-left"}]
]</code></pre>


#####datar参数详细

|参数             |详细                                                 |
|-----------------|----------------------------------------------------|
|css              |你的动画CSS样式,此项有值时，其它参数无效；               |
|style            |style样式，如transform: translateX(100px),每个样式必须以;结束；如CSS有值，此项无效|
|name             |内置动画样式；<br/><code>x-left</code><code>x-right</code><code>y-top</code><code>y-bottom</code><code>scale</code><code>scaleFrom</code><code>scaleX</code><code>scaleY</code><br/>如果css或style有值，此项无效；|
|to               |动画到你设定的样式;默认为false,false时值为from;如CSS有值，此项无效|
|timer            |动画的时间；默认0.5;如CSS有值，此项无效；|
|ease             |样式缓动;默认cubic-bezier(0.165, 0.84, 0.44, 1);如CSS有值，此项无效;|
|delay            |动画的延时;默认0,依照结构递增0.1;如CSS有值，此项无效;|
|delayRewrite     |延时重写;默认false,如果为true,则重写了delay，此项数组后面的delay值为此delay的值递增0.1;如CSS有值，此项无效;|