# ReverseEngineerEasyMoney
A quick rundown of how I was able to reverse engineer an app from 2009

# A little about Easy Money

![](https://i.imgur.com/RePzyD6.png)

This app dates back a way, the last known update was back in 2014 and the original website Handyapps seems to no longer exist. 

Thus with no support and no way to pay for an actual license, the last best approach is to hack inside and see what I can do.

# The approach

## Figure out how to decompile the app.

This quick and dirty method was my first stop shop. You can find it [here](https://forum.xda-developers.com/showthread.php?t=2213985). 

![image](https://user-images.githubusercontent.com/11879769/35950677-f1f18ae4-0c34-11e8-9b6f-b42587410bd0.png)

I found that the guide over-does it in terms of what you actually need. In other words, while you can rigorously follow every single instruction, it seemed that the [documentation](https://ibotpeaches.github.io/Apktool/documentation/) for `apktool` was enough to work with.

# The actual things you'll need

## It's just 3

First off you're going to need to use `Java`. There's no escaping that. Just download the latest JDK and JRE. (I have no clue if JRE is actually even required, I actually doubt it's necessity)

![image](https://user-images.githubusercontent.com/11879769/35950763-4822fa7e-0c35-11e8-8290-7cfcc0987dd3.png)

Second, you'll need `APKTOOL`

![image](https://user-images.githubusercontent.com/11879769/35950854-de34df0a-0c35-11e8-8551-144cebf55b7a.png)

And third and foremost, you're going to need the app (.apk) that you're going to attempt to reverse engineer. In my case `EasyMoney`

# Process towards success

## The process was pretty messy for me personally, but because I'm recounting my experience for you, it's going to be rather short.

### The Download
First off you're going to need to [download](https://bitbucket.org/iBotPeaches/apktool/downloads/apktool_2.3.1.jar) `apktool`. The current version right now is `2.3.1`

### The app
Next you're going to need to get the app off your phone. Use AirDroid to do this.
![0e49d9ee-37e1-461e-bb13-2627695ed94d](https://user-images.githubusercontent.com/11879769/35951443-acfb638e-0c38-11e8-9806-df0a52de3b65.gif)

### The Commands

### The Code
The `smali` code
![image](https://user-images.githubusercontent.com/11879769/35951239-c632fa5c-0c37-11e8-9574-148b45dd2fea.png)

The Java code
![image](https://user-images.githubusercontent.com/11879769/35951216-a8adf4fa-0c37-11e8-8cc2-6e02e6db07ae.png)

## The initial approach

Initially I had foudn that I could decompile the code


After looking at the java code, your first intention may be to do delete everything and have the function return true always. This is easier said than done when it comes to `smali` code. There is really no easy way of changing the smali code and not corrupting your `apk` in the process. Of course, if you're feeling really ambitious there is always learning the Dalvik opcodes. 

Of course in my situation I realized I didn't have much choice and that I'd need to learn Dalvik opcodes.

The second approach would be to somehow have the two `if` statements negated in some way. In other words, never let those statements ever return False. 
![image](https://user-images.githubusercontent.com/11879769/35951633-78c97b2c-0c39-11e8-9c62-38fcb488a636.png)

Again, this is easier said than actually done. The actual `smali` code is very obfuscated, and this may or may not be intentional. If you're like me and wanting to see the java code equivalent instead of the smali code, here's the [resource I used](http://www.javadecompilers.com/apk). You can then download it and see the apk's guts in nice java form. However, buyer beware, you cannot actually recompile the decompiled java files back into an apk. (Unless you're using Android studio... you will have to construct the whole app back together)

After spending time understanding the opcodes and how everything is assigned I realized there was a much easier approach. On line 494, I found that we are always returning the values 

### The Code Result
Changing the `smali` code
![image](https://user-images.githubusercontent.com/11879769/35951200-9cf7c078-0c37-11e8-9c3b-6fe946a864ec.png)

The Java code
![image](https://user-images.githubusercontent.com/11879769/35951248-d761ad8c-0c37-11e8-8717-96f2a05800ed.png)
