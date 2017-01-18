# css3-transition-example
There is a solution to resolve that play css3 transition animation continuously.

Css3 provide two ways to make animation: transition and key-frames. we always use transition.but how can we make a continuous transition animation easily. I create a method like this:

Firstly, save all transition info in a array: 
```javascript
    /**
     * 为了更为细节的动画控制，放弃在html上标记自定义属性形成自定义动画的方法
     * 采用形成动画数组的方式
     * 某一个时间可以有多个同时触发，这个数组最多只是一个二维数组
     *
     * 一个嵌套数组中的元素自定义间隔时间必须相等
     */
    var pages = [
        {       //page1
            'upIn': ['.logo|on|add', '.circle|on|add', ['.circle-icon|on|add','.icon|on|add'], '.slogan|on|add', '.car-run|on|add|200', '.cdj|on|add|200', '.bmw|on|add|200', '.bm320|on|add'],
            'upOut': ['.circle-icon|next|add', '.slogan|next|add|200', '.cdj|on|remove|200', '.bmw|on|remove|200', '.bm320|on|remove', '.car-run|next|add'],
            'downIn': ['.car-run|next|remove|100', '.bm320|on|add|100', '.bmw|on|add|100', '.cdj|on|add|100', '.slogan|next|remove', '.circle-icon|next|remove'],
            'downOut': []
        },
        {       //page2
            'upIn': [ '.niu-ren|on|add', 'body|zoom-out|add', '.mail-icon|on|add', '.words|on|add', '.money|on|add', 'body|zoom-out|add' ],
            'upOut': ['.niu-ren|next|add', '.mail-icon|next|add', '.words|next|add'],
            'downIn': ['.words|next|remove', '.mail-icon|next|remove', '.niu-ren|next|remove'],
            'downOut': ['.words|on|remove', '.mail-icon|on|remove', '.niu-ren|on|remove']
        },
        {       //page3
            'upIn': ['.p3-title|on|add', '.recommend|on|add|100', '.recommend-name|on|add|100', '.recommend-tel|on|add|100', '.recommend-mail|on|add|100', '.be-recommend|on|add|100', '.be-recommend-name|on|add|100', '.be-recommend-job|on|add|100', '.be-recommend-resume|on|add|100'],
            'upOut': ['.p3-title|next|add|200', '.recommend|next|add|200', ['.recommend-name|next|add|200', '.recommend-tel|next|add|200', '.recommend-mail|next|add|200'], '.be-recommend|next|add|200', ['.be-recommend-name|next|add|200', '.be-recommend-job|next|add|200', '.be-recommend-resume|next|add|200']],
            'downIn': ['.be-recommend-resume|next|remove|200', '.be-recommend-job|next|remove|200', '.be-recommend-name|next|remove|200', '.be-recommend|next|remove|200', '.recommend-mail|next|remove|200', '.recommend-tel|next|remove|200', '.recommend-name|next|remove|200', '.recommend|next|remove|200', '.p3-title|next|remove|200'],
            'downOut': ['.be-recommend-resume|on|remove|200', '.be-recommend-job|on|remove|200', '.be-recommend-name|on|remove|200', '.be-recommend|on|remove|200', '.recommend-mail|on|remove|200', '.recommend-tel|on|remove|200', '.recommend-name|on|remove|200', '.recommend|on|remove|200', '.p3-title|on|remove|200']
        },
        {       //page4
            'upIn': ['.p4-title|on|add', '.java|on|add|100', '.android|on|add|100', '.ios|on|add|100', '.view-design|on|add|100', '.marketing|on|add|100', '.product|on|add|100', '.query-btn|on|add', '.email|on|add'],
            'upOut': [],
            'downIn': [],
            'downOut': ['.email|on|remove|100', '.query-btn|on|remove|100', '.product|on|remove|100', '.marketing|on|remove|100', '.view-design|on|remove|100', '.ios|on|remove|100', '.android|on|remove|100', '.java|on|remove|100', '.p4-title|on|remove']
        }
    ];
```

Secondly, resolve this info object
```javascript
   /**
     * 显示动画
     * @param container page容器
     * @param aniArr 动画信息数组
     * @param index 递归标记
     */
    function showAnimation(container, aniArr, index, endCall){
        clearTimeout(prevTimer);
        if(index >= aniArr.length){ //递归结束条件
            endCall = endCall || function(){};
            endCall();
            return;
        }

        var information = aniArr[index];
        var tmpl;   //标记自定义时间，数组中的默认第一个元素
        if( typeof information == 'string'){
            handleClass(information, container);
            tmpl = information.split('|')[3];
        } else {
            //infos是数组
            for(var j= 0,jLen=information.length; j<jLen; j++){
                handleClass(information[j], container);
            }
            tmpl = information.length > 0 ? information[0].split('|')[3] : undefined;
        }

        prevTimer = setTimeout(function(){
            showAnimation(container, aniArr, ++index, endCall);
        }, !!tmpl&&tmpl.trim().length>0 ? parseInt(tmpl) : 500);   //没有自定义时间
    }
```
you can download and run index.html in browser to see the effect.
