# SunnySide-Game
<html>
	<head>
	<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
		<style>
		
			canvas {
				border:1px solid #d3d3d3;
				background-color: #f1f1f1;
			}
			
			button {
				margin:13px;
				color:black;
				height:40px;
				width: 40px;
				background-color:lightblue;
				padding: 14px 20px;
						
			}
			
		</style>
		
	</head>
	
	<body onload = "startGame()">
	
		<script>
alert("hi");
			var myDog, time, myBackground, a, b, c, radius, left, right, top, bottom, otherLeft, otherRight, otherBottom, otherTop, crash, ctx; 

			var obstacles = [];
			
		function startGame() {
			alert("test");
				myDog = new component(130, 80, "huskyDrawing.png", 20, 180, "image");
				myBackground = new component(1510, 730, "snow.jpg", 0, 0, "backgroundPic");
				time = new component("30px", "Consolas", "black", 30, 30, "text");
				obstacles = new component(300, 300, "SunnySide_Shrub.png", 400, 350, "obstacle");

				myGameArea.start();		
			}	
			
			
			
			var myGameArea = {
				canvas : document.createElement("canvas"),
				start : function() {
					this.canvas.width = 480;
					this.canvas.height = 270;
					this.context = this.canvas.getContext("2d");
					document.body.insertBefore(this.canvas, document.body.childNodes[0]);
					this.frameNo = 0;
					this.interval = setInterval(updateGameArea, 20);
					},
				clear : function() {
					this.context.clearRect(0, 0, this.canvas.width, this.canvas.height);
				},
				stop : function() {
					clearInterval(this.interval);
				}
			}
			
			function component(width, height, color, x, y, type) {
			
				this.type = type;
				this.width = width;
				this.height = height;
				this.speedX = 0;
				this.speedY = 0;    
				this.x = x;
				this.y = y; 
				
				this.update = function() {
				
					ctx = myGameArea.context;

					if (type == "image"||type == "backgroundPic"|| type == "obstacle") {
					this.image = new Image();
					this.image.src = color;
						ctx.drawImage(this.image, this.x, this.y, this.width, this.height);
						
						      if (type == "backgroundPic") {
							  
								ctx.drawImage(this.image, this.x + this.width, this.y, this.width, this.height);
								
							  }
							  
					this.update = function() {
						ctx = myGameArea.context;
						if (this.type == "text") {
						  ctx.font = this.width + " " + this.height;
						  ctx.fillStyle = color;
						  ctx.fillText(this.text, this.x, this.y);
						} else {
						  ctx.fillStyle = color;
						  ctx.fillRect(this.x, this.y, this.width, this.height);
						}
					}
				}
				
				this.newPos = function() {
				
					this.x += this.speedX;
					this.y += this.speedY; 
					
					if (this.type == "backgroundPic") {
					
					  if (this.x == -(this.width)) {
					  
						this.x = 0;
					  }
					}
				}
				
				this.crashWith = function(otherobj) {
				
					var myleft = this.x;
					var myright = this.x + (this.width);
					var mytop = this.y;
					var mybottom = this.y + (this.height);
					var otherleft = otherobj.x;
					var otherright = otherobj.x + (otherobj.width);
					var othertop = otherobj.y;
					var otherbottom = otherobj.y + (otherobj.height);
					var crash = true;
					
					if ((mybottom < othertop) || (mytop > otherbottom) || (myright < otherleft) || (myleft > otherright)) {
					
						crash = false;
						
					}
					return crash;
				}
			}
			
			function updateGameArea() {
			
				var x, gap1, height, gap, minHeight, maxHeight, minGap, maxGap, y, width, minWidth, minHeight;
				
				for (i = 0; i < obstacles.length; i += 1) {
				
					if (myDog.crashWith(obstacles[i])) {
					
					  myGameArea.stop();
					  return;
					  
					} 	
				}
				  
				myGameArea.clear();
				myGameArea.frameNo += 1;
				
				if ((myGameArea.frameNo == 1 )|| (everyInterval(120)) || (type=="obstacle") || (type == "image")) {

					x = myGameArea.canvas.width;
					minHeight = 20;
					maxHeight = 80;
					height = Math.floor(Math.random()*(maxHeight-minHeight+1)+minHeight);
					minGap = 100;
					maxGap = 200;
					gap = Math.floor(Math.random()*(maxGap-minGap+1)+minGap);
				
					obstacles.push(new component(10, height, "SunnySide_Shrub.png", x, 0, "obstacle")); //adds component properties to the end of the array
					obstacles.push(new component(10, x - height - gap, "SunnySide_Shrub.png", x, height + gap, "obstacle"));
					
				}
		for (i = 0; i < obstacles.length; i += 1) {
				
					obstacles[i].x += 1; //chooses which obstacles property to display
					obstacles[i].newPos();
					obstacles[i].update(); //updates and adds new one to canvas
					
				}
				
				time.text = "SCORE: " + myGameArea.frameNo;
				time.update();
				myDog.newPos();    
				myDog.update();	
				
				myBackground.speedX = -1;
				myBackground.newPos();    //so the background image is continuous
				myBackground.update();
				
			}

			function everyInterval(n) {
			
			  if ((myGameArea.frameNo / n) % 1 == 0) {
				return true;
			  }
			  return false;
			}
			
			function moveup() {
			
				myDog.speedY = -2; 
				
			}

			function movedown() {
			
				myDog.speedY = 2; 
				
			}

			function moveleft() {
			
				myDog.speedX = -2; 
				
			}

			function moveright() {
			
				myDog.speedX = 2; 
				
			}
			
			document.onkeydown = function(evt) { //connecting the dog to arrows on keyboard
			
				evt = evt || window.event; 
				
				switch (evt.keyCode) { 
				
					case 37:
					moveleft(); 
					break; 
					 
					case 38:
					moveup(); 
					break;
					
					case 39:
					moveright(); 
					break; 
					
					case 40:
					movedown(); 
					break;
				
				} 
				
			};
			
		</script>
		
	</body>
	
</html>
