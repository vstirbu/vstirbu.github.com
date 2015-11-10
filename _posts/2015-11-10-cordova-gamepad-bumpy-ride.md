---
layout: post
title: "Bumpy Ride with Cordova and Android Gamepads"
tags: cordova javascript gamepad taekwon-do
---

## The background

Back in 2012, I have started working on a scoring system for Taekwon-Do competitions. A key part of the project was to enable the judges to score without being distracted from watching what is really important: _the action in the ring_... 

After some consideration, which dived also into exploring custom hardware, we decided to use [JDX S5110](https://en.wikipedia.org/wiki/JXD_S5110) Android gamepads for this purpose. Thanks to [Apache Cordova](http://cordova.apache.org), I can easily create the user interface using familiar web technologies, and also have the option to extend the functionality of the application with native features.

## The quick and dirty GamepadButtons plugin

These were the Ice Cream Sandwich days, when the WebView lacked support for [Gamepad API](http://www.w3.org/TR/gamepad/). I had to roll my sleeves and start looking for a solution. A quick search on Google revealed that while there were a number of polyfills for desktop browsers, none was available for mobile browsers, especially for WebViews...

Thanks to good documentation I was able to fix the issue by capturing the gamepad button press or release events, and expose them inside the JavaScript runtime using the [GamepadButtons](https://github.com/vstirbu/GamepadButtons) plugin. The key was to set up a key listener on the WebView, and initialize it on application startup:

{% highlight java %}
public class Gamepad extends CordovaPlugin {
  
  @Override
  public void initialize(CordovaInterface cordova, CordovaWebView webView) {
    
    this.webView.setOnKeyListener(new OnKeyListener() {
      
      @Override
      public boolean onKey(View v, int keyCode, KeyEvent event) {
        //send the event to JavaScript
      }
      
    });
  }
}
{% endhighlight %}

As I needed the buttons for recoding the scores I chose to implement a subset of the Gamepad API proposed by Mozilla, which had extra events such as `gamepadbutton`, leaving out the actual API needed for playing games.

This being done I could focus on more important issues...

## A more mature Cordova platform but still...

Fast forward to 2015 and the original gamepad was replaced with a new version: [JDX S5110B](http://www.jxd.hk/game-console/s5110b/). Despite what the name implies this is a completely new device and... surprise, surprise... the plugin does not deliver the gamepad events anymore.

Cordova ecosystem made huge progress starting with formalizing the plugin API, moving the plugin registry to `npm`, but also supporting additional web views like [crosswalk](https://crosswalk-project.org), besides the one bundled with Android.

These changes were not good news for my neglected plugin. As a result of supporting multiple web views, the API that allowed my keys to be delivered to the web engine was [removed](https://issues.apache.org/jira/browse/CB-9005). Luckily, Android documentation on handling [gamepads](http://developer.android.com/training/game-controllers/controller-input.html) has clear enough instructions.

So, I'm down to two options. The first one, which is the more frendlier for a Cordova plugin, is to implement the event handlers in the `View`, but the gamepad events were handled first by the Chromium based WebView and I'm left with nothing...

The second option is to handle the events in the `Activity`. The approach involves extending `CordovaActivity` with a handler for intercepting all key events. If the key is coming from a keypad or a joystick they are passed to the plugin, otherwise they are processed normally:

{% highlight java %}
public class CordovaGamepadActivity extends CordovaActivity {

  @Override
  public boolean dispatchKeyEvent(KeyEvent event) {
    if ((event.getSource() & InputDevice.SOURCE_GAMEPAD) == InputDevice.SOURCE_GAMEPAD
      || ((event.getSource() & InputDevice.SOURCE_JOYSTICK) == InputDevice.SOURCE_JOYSTICK)) {

      this.appView.getPluginManager().postMessage("gamepad-plugin", event);
      return true;
    } else {
      return super.dispatchKeyEvent(event);
    }
  }
}
{% endhighlight %}

## The missing parts

Although handling the key events works as expected, the overall experience is not as nice as it should be.

First, the approach of extending the CordovaActivity in the main application file is not liked by [`cordova-cli`](https://github.com/apache/cordova-cli) which expects the `MainActivity` class to extend __[only](https://github.com/apache/cordova-lib/blob/master/cordova-lib/src/cordova/metadata/android_parser.js#L267)__ `CordovaActivity`. This prevents building the application using command line:

{% highlight bash %}
$ cordova build android
Error: No Java files found which extend CordovaActivity.
{% endhighlight %}

Secondly, the Gamepad API implementation is so limited that maybe is better to change the plugin name, but this is something that can be fixed...

Thanks for reading so far, and as always... [pull requests]((https://github.com/vstirbu/GamepadButtons)) are welcome!!! 
 