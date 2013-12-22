---
layout: post
title: "itemtemplate selection by type in xaml only"
date: 2009-09-23 19:29
comments: true
---
some days ago i found my self in need to change the itemtemplate in a listbox according to their type. and if with less code as possible. i had done this in a treeview but i couldnt find the code again. so i needed to figure it out again and with some help from a collegue we ended with this:
``` xml
	<!-- more xaml here -->
	<itemscontrol itemssource="{Binding Items}">  
		<itemscontrol.resources>    
			<datatemplate datatype="{x:Type MyNameSpace:MyType1}">      
				<radiobutton />    
			</datatemplate>    
			<datatemplate datatype="{x:Type MyNameSpace:MyType2}">      
				<combobox />    
			</datatemplate>    
			<datatemplate datatype="{x:Type MyNameSpace:MyType3}">      
				<textbox />    
			</datatemplate>  
		</itemscontrol.resources>
	</itemscontrol>
		<!-- more xaml here -->
```

**remark**: you arent allowed to give a key to that resources. i suppose the type is kind of key here. 