---
layout: blogpost
title: How To Create A Simple Counter Using Javascript, HTML & CSS
comments: true
---

So I was a little bored and decided to play around with Javascript. The idea was to create something like the counter on [this page](https://www.greencirclesalons.ca/) using javascript, HTML and a little CSS; using a different layout. I guess it looks nice, right?. 
I figured someone might find this useful so I decided to share the snippet here.
The final output is as shown in the image above and increments from the left.
And the snippet in action is available on this [JS fiddle](https://jsfiddle.net/hmatrix/v3jncqac/).
### The HTML
Saved as index.html
```
<!DOCTYPE html>
<html>
	<head>
		<title>JS Counter</title>
		<link rel="stylesheet" type="text/css" href="style.css">
	</head>

	<body onload="incrementCount(10)">
		<div class="main_container" id="id_main_container">
			<div class="container_inner" id="display_div_id">
			</div>
		</div>
	</body>
</html>
```
### The CSS
Saved as style.css

```
.main_container {
	height: 46px;
	width: auto;
	padding: 3px;
	margin: 2px;
	max-width: 300px;
	background-color: #555555;
	align-content: center;
}
.container_inner {
	height: auto;
	border: none;
	background-color: #555555;
	max-width: 290px;
	vertical-align: center;
	padding-top: 12px;
	padding-left: 10px;
	align-content: center;
}
 .num_tiles {
	width:30px;
	height: 30px;
	background-color: #888888;
	color: #ffffff;
	font-size: 22px;
	text-align: center;
	line-height: 20px;
	padding: 3px;
	margin: 1.5px;
	font-family: verdana;
	vertical-align: center;
}
```
### The Javascript
```
<script>
	var counter_list = [10,10000,10000];
	var str_counter_0 = counter_list[0];
	var str_counter_1 = counter_list[1];
	var str_counter_2 = counter_list[2];
	var display_str = "";
	var display_div = document.getElementById("display_div_id");

	function incrementCount(current_count){
		setInterval(function(){
			// clear count
			while (display_div.hasChildNodes()) {
			    display_div.removeChild(display_div.lastChild);
			}
			str_counter_0++;
			if (str_counter_0 > 99) {
				str_counter_0 = 10; // reset count
				str_counter_1++;    // increase next count
			}
			if(str_counter_1>99999){
				str_counter_2++;
			}
			display_str = str_counter_2.toString() + str_counter_1.toString() + str_counter_0.toString();
			for (var i = 0; i < display_str.length; i++) {
				var new_span = document.createElement('span');
				new_span.className = 'num_tiles';
				new_span.innerText = display_str[i];
				display_div.appendChild(new_span);
			}
		},1000);
	}
</script>
```
You can play around with the values in the `counter_list` variable to change the display on the screen.
The time interval set using Javascript's `setInterval()` function is currently set at 1000 microseconds(1 second), you can also alter this to change the counting interval.
The HTML and JS codes are in the same file such that final `index.html` file is as shown below.
```
<!DOCTYPE html>
<html>
	<head>
		<title>JS Counter</title>
		<link rel="stylesheet" type="text/css" href="style.css">
	</head>

	<body onload="incrementCount(10)">
		<div class="main_container" id="id_main_container">
			<div class="container_inner" id="display_div_id">
			</div>
		</div>
	</body>
</html>

<script>
	var counter_list = [10,10000,10000];
	var str_counter_0 = counter_list[0];
	var str_counter_1 = counter_list[1];
	var str_counter_2 = counter_list[2];
	var display_str = "";
	var display_div = document.getElementById("display_div_id");

	function incrementCount(current_count){
		setInterval(function(){
			// clear count
			while (display_div.hasChildNodes()) {
			    display_div.removeChild(display_div.lastChild);
			}
			str_counter_0++;
			if (str_counter_0 > 99) {
				str_counter_0 = 0; // reset count
				str_counter_1++;    // increase next count
			}
			if(str_counter_1>99999){
				str_counter_2++;
			}
			display_str = str_counter_2.toString() + str_counter_1.toString() + str_counter_0.toString();
			for (var i = 0; i < display_str.length; i++) {
				var new_span = document.createElement('span');
				new_span.className = 'num_tiles';
				new_span.innerText = display_str[i];
				display_div.appendChild(new_span);
			}
		},1000);
	}
</script>
```

I hope it helps someone out there.
***Happy Coding!***
