# RetroSnaker
这是我写的第一个canvas的小游戏——贪吃蛇，界面很简单，就是画很多的正方形连在一起当成一条蛇

##代码
有一个bug，当在快速按下多个方向键时会咬到自己，找了很久翻出来的代码了，有兴趣的可以去找找原因哟
````javascript
<!DOCTYPE HTML>
<html>
  <head>
<title>贪吃蛇</title>
<script src="js/jquery-1.10.1.min.js" type="text/javascript"></script>
<script type="text/javascript">	
//我的思路，用一个数组记录蛇的每一节，每移动一次画一个头，把尾去掉
 $(document).ready(function(){	
	var chi=0;//用来判断蛇是否吃到食物，吃到为1，没吃到为0
	var dai;//用来保存蛇尾的位置
	var she=[];//蛇的数组
	she[0]=[50,50];//蛇的初始位置		

	var a=parseInt(Math.random()*(30))*10;//随机产生第一个食物的x点
	var b=parseInt(Math.random()*(30))*10;//随机产生第一个食物的y点
	var direction=1;//键盘监听的初始值，1往右，2往下，3往左，4往上
	var c=document.getElementById("myCanvas");
	var ctx=c.getContext("2d");
	ctx.fillRect(she[0][0],she[0][1],10,10);//画蛇头，只有一个头
	//画食物
    ctx.fillStyle="green";
	ctx.fillRect(a,b,10,10);
	//键盘方向键监听
	$(document).keydown(function(event){ 
		if(event.keyCode == 37&&direction!=1){ 
		   direction=3;
		}else if (event.keyCode == 39&&direction!=3){  
		   direction=1;
		}else if (event.keyCode == 38&&direction!=2){ 
		   direction=4;
		}else if (event.keyCode == 40&&direction!=4){ 
		   direction=2;
		} 
	});	
	//蛇
   function canvasDraw(){
     //撞墙判断
       if(she[0][0]>290||she[0][0]<0||she[0][1]>290||she[0][1]<0){		
		   alert("撞墙了");
		   clearInterval(start);  
		}
		var l=she.length;//蛇的长度       
		ctx.beginPath();
		ctx.fillStyle="black";
		//如果蛇没吃到食物，吧蛇尾去掉
		if(chi==0){
		   ctx.clearRect(she[l-1][0],she[l-1][1],10,10);
		}
	    
		dai=she[l-1];//把蛇尾的信息赋给他
		//如果蛇超过两截以上，从蛇头开始把数组往后移动一位，不要蛇尾
		if(l>1){	
		  for(var i=0;i<l-1;i++){
		      she[l-i-1]=she[l-i-2];
		  }	
		}		      		
		//按下键盘方向后蛇的方向
		if(direction==1){she[0]=[she[0][0]+10,she[0][1]];}
		else if(direction==2){she[0]=[she[0][0],she[0][1]+10];}
		else if(direction==3){she[0]=[she[0][0]-10,she[0][1]];}
		else if(direction==4){she[0]=[she[0][0],she[0][1]-10];}
		//判断咬到自己
		for(var i=1;i<l;i++){
		      if(she[0][0]==she[i][0]&&she[0][1]==she[i][1]){
			     alert("你个傻逼，咬到自己啦！");
				 clearInterval(start); 
			  }
		 }		 
		ctx.fillRect(she[0][0],she[0][1],10,10);//移动时画一个蛇头
		
	    chi=0;
		var c=1;
		//吃到食物时随机产生另一个食物
		if(she[0][0]==a&&she[0][1]==b){     	
		  she[l]=dai;
		  chi=1;
		  while(c==1){
			  a=parseInt(Math.random()*(30))*10;
			  b=parseInt(Math.random()*(30))*10;
			  for(var i=0;i<l+1;i++){
			      if(a!=she[i][0]||b!=she[i][1]){ c=0;}	 
			  }		     
		  }	  
		  ctx.fillStyle="green";
	      ctx.fillRect(a,b,10,10);
        }
		context.closePath();
	}
	//每150毫秒执行一次这个函数
	var start=setInterval(canvasDraw,120);
})
</script>
</head>
<body>
<div style="margin:0 auto;margin-top:150px;background:#dbdbdb;width:300px;">
<canvas id="myCanvas" width="300" height="300" >
</canvas>
</div>
</body>
</html>
```
