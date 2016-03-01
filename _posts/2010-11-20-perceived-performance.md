---
comments: true
date: 2010-11-20 21:13:21
layout: post
title: Perceived performance
categories: [design, development, wp7]
---

I’m watching a recording of Windows Phone Design Days, it’s the part about “perceived performance” with Jaime Rodriguez. He put up a nice table with response times and recommended visual feedback. I’ve seen these recommendations before. I’ve talked about these recommendations with my team. I’m insisting applying them every time when I see in out apps that an action has no visual response within one second after triggering it. 

Even if it’s talking in context of User Experience on Windows Phone, I think these numbers are a formal guidance for good user experience on every device, modern gadget or software application.

I’ll make a copy of this table here, just to keep it in a handy place for further reference. It’s so well worth keeping it...

<table border="1">
	<tr>
		<th>system response time</th>
		<th>duration</th>
		<th>recommended feedback</th>
	</tr>
	<tr>
		<td>instantaneous response</td>
		<td>bellow 100 milliseconds</td>
		<td>built in common controls</td>
	</tr>
	<tr>
		<td>immediate response</td>
		<td>between 500 milliseconds and 1 second</td>
		<td>built in common controls</td>
	</tr>
	<tr>
		<td>very short wait</td>
		<td>2 seconds or less</td>
		<td>built in common controls</td>
	</tr>
	<tr>
		<td>short wait</td>
		<td>2 to 5 seconds</td>
		<td>progress bars</td>
	</tr>
	<tr>
		<td>moderate wait</td>
		<td>5 to 10 seconds</td>
		<td>progress bar with details like percentage completition or time remaining</td>
	</tr>
	<tr>
		<td>long wait</td>
		<td>10 seconds or greater</td>
		<td>progress bar with details of sub steps. Allow user to move to other tasks</td>
	</tr>
	<tr>
		<td>continuous feedback</td>
		<td>long process</td>
		<td>progress bar with details of sub steps. Allow user to move to other tasks</td>
	</tr>
</table>


Read, remember & **apply!**
