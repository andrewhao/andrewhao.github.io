---
author: andrewhao
comments: true
date: 2007-03-28 20:50:00
layout: post
slug: verilogical
title: Verilogical
wordpress_id: 728
categories:
- Andrew 2.0
---

Oftentimes in [Verilog](http://en.wikipedia.org/wiki/Verilog) code, we build testbenches (test fixtures). A common thing to do is to instantiate a module and plug in wires of the same names.  
  
Now a common hack is to copy and paste the module declaration line (the "prototype," if you will). The prototype looks like this:  




> module 	Foo(	bar,<br></br>		baz,<br></br>		...<br></br>	);	<br></br>



The HDL synthesizer can correctly wire inputs, regardless of instantiated order, by surrounding the names of the local wires with the input names as declared in the prototype in a format such as:  



> Foo foo_instance (	.bar(bar),<br></br>			.baz(baz),<br></br>			...<br></br>		);<br></br>



So to do so is a rather painful matter of adding a period before the input name, copy and pasting the input name and pasting it after the name, bracketed by parentheses.

But what do you do if your module has thirty inputs? Our previous solution was to surrender our wrists to inevitable Repetitive Stress Injuries. Today, I realized that I should have used a freakin' elementary [regular expression](http://en.wikipedia.org/wiki/Regular_expression).



> Search:		(.*),<br></br>Replace:	.$1($1),<br></br>


Wah-lah! I'm kicking myself for not thinking of this earlier. And it has taken me 10 minutes to tell you how I saved forty-five seconds and my poor wrists. Anything to avoid more work (shh, don't tell Alex) :)  


[![](http://ec1.images-amazon.com/images/P/B00004XONN.01._SCTHUMBZZZ_.jpg)](http://www.xanga.com/Amazon/Click.aspx?asin=B00004XONN&user=378399)
**Currently Listening**  
[**Kid A**](http://www.xanga.com/Amazon/Click.aspx?asin=B00004XONN&user=378399)  
By Radiohead  
[see related](http://www.xanga.com/Amazon/Click.aspx?asin=B00004XONN&user=378399&related=1)
