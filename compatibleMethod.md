*获取元素的计算后样式属性*
```
function getStyle(element,attr){
  if(element.currentStyle){
    return element.currentStyle[attr];
  }else{
    return window.getComputedStyle(element,null)[attr];
  }
}
```
*scroll().top or scroll().left获取已经滚动到元素的左边界或上边界的像素数*
```
function scroll(){
  return{
    top:window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop || 0;
    left:window.pageXOffset || document.documentElment.scrollLeft || document.body.scrollLeft || 0;
  };
//return值是一个json，包含两个键分别是top和left
}
```
*获取可视窗口大小*
```
function client(){
  return{
    width:window.innerWidth || document.documentElement.clientWidth || document.body.clientWidth || 0;
    height:window.innerHeight || document.documentElement.clientHeight || document.body.clientHeight || 0;
  };
}
```
*监听事件兼容处理*
```
function eventListener(obj,type,handler,unbind){
  if(unbind === true){
    if(obj.removeEventListener){
      obj.removeEventListener(type,handler);//dom标准
    }else{
      obj.detachEvent("on"+type,handler);//ie
    };
  }else{
    if(obj.addEventListener){
      obj.addEventListener(type,handler);//dom标准
    }else{
      obj.attachEvent("on"+type,handler);//ie
    };
  };
};
```

*匀速动画*
```
function animate(obj,attr,distance){
  var step=20;
  var leader=parseInt(getStyle(obj,attr)) || 0;
  clearInterval(obj.atimer);
  obj.atimer=setInterval(function(){
    distance>leader?leader+=step:leader-=step;
    if(Math.abs(distance-leader)>step){
      obj.style[attr]=leader+"px";
    } else{
      obj.style[attr]=distance+"px";
      clearInterval(obj,atimer);
    }
  },15)
}
```

*进度条*
```
function Progress(id,width,height,outClass,inClass){
  this.width=width;
  this.height=height;
  this.color="#fff";
  this.progress=document.createElement("div");
  this.percentage=documentcreatElement("div");
  this.filler=document.createElement("div");
  var element=document.getElementById(id);
  if(width){//如果没有传入宽度则默认宽度为200px
    this.progress.style.width=this.width+"px";
  }else{
    this.progress.style.width="200px";
  }
  
  if(height){//如果没有传入高度则默认高度为20px
    this.progress.style.height=this.height+"px";
  }else{
    this.progress.style.height="20px";
  }
  
  if(typeof outClass === "string" && (/^[a-zA-Z](\w|[-])+$/g.test(outClass))){
    this.progress.className=outClass;
  }else{
    this.progress.style.border="1px solid #ccc";
    this.progress.style.backgroundImage="linear-gradient(to bottom,#ccc 0%,#fff 40%,#ccc 100%)";
    this.progress.style.borderRadius="10px";
  }
  
  this.progress.style.overflow="hidden";
  this.progress.style.position="relative";
  element.appendChild(this.progress);
  
  this.progress.appendChild(this.percentage);
  this.percentage.style.width="100%";
  this.percentage.style.height="100%";
  this.percentage.style.textAlign="center";
  this.percentage.style.position="absolute";
  this.percentage.innerHTML="0%";
  
  this.progress.appendChild(this.filler);
  this.filler.style.height="100%";
  this.filler.style.width=0;
  if(typeof inClass === "string" && (/^[a-zA-Z](\w[-])+$/g.test(inClass))){
    this.filler.className=inClass;
  }else{
    this.filler.style.backgroundColor="#DC7BBE";
    this.filler.style.backgroundImage="linear-gradient(to bottom,#0af 0%,#0fff 40%,#0af 100%)";
  }
}

function fill(value){
  if(value){
    this.percentage.innerHTML=value+"%";
    this.percentage.style.color=this.color;
    value=(this.progress.offsetWidth-2)/100*value;
    this.filler.style.width=value+"px";
  }else{
    this.filler.style.width=0;
    this.percentage.innerHTML="0%";
  }
}
```


