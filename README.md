
# 歌词案例

本例主要对lrc歌词进行分割，然后对歌词时间和播放时间进行对比，选择是否显示歌词。


### 最开始的歌词算法
```javascript
 function showLrc() {
        for (let i = 0; i < result.length-1; i++) {
            let curTime = myAudio.currentTime;//获取当前的播放时间
            if ((curTime >result[i][0]) && (curTime<result[i+1][0]) && document.getElementById("lyric").innerHTML !== result[i][1]) {
                //播放时间大于对应歌词时间小于下一句歌词时间就显示当前歌词
                document.getElementById("lyric").innerHTML = result[i][1];
                break;//找到对应歌词就停，不停的话，呵呵。。。
            }
        }
    }
```
会过量消耗CPU

### 改进后的代码
解决过度循环
```javascript
function showLrc() {
        let curTime = myAudio.currentTime;//获取当前的播放时间
        if(flagN<result.length-1){
            if ((curTime >result[flagN][0]) && (curTime<result[flagN+1][0]) && document.getElementById("lyric").innerHTML !== result[flagN][1]) {
                document.getElementById("lyric").innerHTML = result[flagN][1];
                flagN = flagN+1;
            }
        }else{
            if(curTime >result[flagN][0]){
                document.getElementById("lyric").innerHTML = result[flagN][1];
                if(flagN === result.length-1) flagN = 0;
            }
        }
    }
```
想去掉条件语句
### 更优化的代码
解决去掉条件语句，需要对歌词进行稍微改造一下，在歌词最后加一个空的歌词时间 例如 ’[04:34.34] '.
```javascript
  function showLrc() {
        let curTime = myAudio.currentTime;//获取当前的播放时间
        if ((curTime >result[flagN][0]) && (curTime<result[flagN+1][0]) && document.getElementById("lyric").innerHTML !== result[flagN][1]) {
            document.getElementById("lyric").innerHTML = result[flagN][1];
            flagN = (flagN===result.length-2) ? 0 : flagN+1;
        }
    }
```

# 核心问题
主要是解决歌词正常显示的时候，还要做到减少后台计算，较少过度占用CPU资源，优化无处不在，嘻嘻.
