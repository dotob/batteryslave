---
layout: post
title: "what is wrong with focus in wpf"
date: 2009-05-14 20:13
comments: true
---
lately we wanted to set the initial focus on a window in wpf to improve the  keyboard experience in our app. what we want to achieve is just the initial  focus. so when one opens the window he can type or push the most meaningful  button instantly. unfortunately we encountered several problems. at first lets  see what wpf offers to set the focus. there are at least 4 possibilities:

- FocusManager
- Keyboard.Focus
- InputElement.Focus
- InputElement.MoveFocus

what i understand from [msdn](http://msdn.microsoft.com/en-us/library/aa969768.aspx) is that  the best way to set the initial focus is the focusmanager. but we encountered two problems where that wont work:
- set initial focus to a button with a commandbinding
- set initial focus to an element defined through a contenttemplate

i did not understand why that is. but i found two strange things: when  setting a breakpoint in the ContentRendered-Event you can see the button is  visible, but it is disabled. so i understand that i doesnt make sense to set the focus to a disabled element. this seems like a bug to me. 

another thing i could not understand is when the content of the control is set through a ContentTemplate the Method FrameworkElement.FindName(string elementName) wouldnt  find the element. isnt that a bug too? when you inspect the window on screen  with scoop u can see all the elements of course.

i tried to use the other focus-methods in various events but none worked. funny thing: if you set the  focus and have a breakpoint in the Loaded-Event and just go on after stopping there the focus is correct. Racecondition?

so is there someone who had similar problems? am i wrong with my feeling that these are bugs?