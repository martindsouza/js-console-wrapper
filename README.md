# Console Wrapper #
  * [Introduction](#Introduction)
    * [Demo](#Demo)
  * [Setup](#Setup)
    * [Download](#Download)
  * [Instructions](#Instructions)
    * [APEX Integration](#APEX_Integration)
  * [Additional Resources](#Additional_Resources)
  * [Change Log](#Change_Log)

# Introduction #
Console is a command in JavaScript (JS). It allows you to print objects to the console window to assist with debugging your JS code.  Console is available in most browsers except IE. Since it is not accessible in IE, developers have to remove it each time they want to move their code into production since most web applications will have some IE clients.

The console wrapper resolves the following issues:

- Will not raise an error if run in IE (null op run)
- Can toggle console output via the [Levels](#Levels.md) option
- Tight integration with [Integration Oracle Application Express (APEX)](#APEX.md)

If you want some more information about console and its capabilities please see the
[Additional Resources](#Additional_Resources) section below.

Console wrapper is developed by [Martin Giffy D'Souza](http://www.talkapex.com).

## Demo ##
You can view some of the console wrapper functions [here](http://apex.oracle.com/pls/apex/f?p=16406:1200:0)

# Setup #
The console wrapper leverages [jQuery](http://www.jquery.com). To load the console wrapper include both [jQuery](http://www.jquery.com) and the [`$console_wrapper.js`]($console_wrapper.js) file:

```html
<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.4.4/jquery.min.js" type="text/javascript"></script>
<script src="$console_wrapper.js" type="text/javascript"></script>
```

## Download ##
You can download the latest release of `$console_wrapper.js`:

  * [$console\_wrapper.js]($console_wrapper.js)
  * [Demo](console_wrapper_demo.html)


# Instructions #

## Levels ##
The console wrapper allows you to control the type of messages that are displayed in the console window. There's a hierarchy of levels that control this. They are:

  * exception
  * error
  * warn
  * info
  * log / debug
  * off

If you set the level to be _info_ all info and above (_warn, error, exception_) messages will be displayed. By default the initial level is set to _off_ which means no messages will be displayed on the console window (_this is slightly different if used in an APEX application, see [APEX Integration](#APEX_Integration)_). You can change the logging level by using the `setLevel` function:

```javascript
//Enable logging mode
$.console.setLevel('log');
```

If you want to see the current level for the console wrapper you can use the `getLevel` command:

```javascript
//Get current level (text)
$.console.getLevel();
```

## jQuery Chained Logging ##
Since the console wrapper uses jQuery you can leverage jQuery's [chaining](http://blog.pengoworks.com/index.cfm/2007/10/26/jQuery-Understanding-the-chain) capabilities as shown in the following example:

```javascript
//This assumes you have some tag with a class of "red".
//For example <span class="red">some text</span>
//This will log something, change the color of the text to red, then log something else
//The main advantage is that it's all done in one line.
$('.red').log('Before Red').css({color:'red'}).log('After Red');
```

When chaining in jQuery it only accepts the _.log()_ function. If you want to display a different type of message set the first parameter in as the type you want. For example:

```javascript
//This is based on the previous example except that it will be a info message instead of a log message
$('.red').log('info','Before Red').css({color:'red'}).log('info','After Red');
```

## Log Parameters ##
You may want to log all the parameters that are passed into a function. The `logParams` function will log all the parameters that were passed into the function along with the variable they were assigned to. For unassigned parameters "unassigned" will appear. The following example highlights how to use the `logParams` function.

```javascript
function myFn(pA, pB){
  $.console.logParams();
  //do something as part of function
}//myFn

//Run myFn
myFn(1,2,3);
```

The output from this is:

```javascript
Parameters (this is grouped)
pA: 1
pB: 2
unassigned: 3
```

The `logParams` function takes an optional `pOptions` [JSON](http://www.json.org/) object. This object supports the following options:

  * **createGroup** _boolean_ If true a group will be created (and closed appropriately). Default: true.
  * **groupCollapsed** _boolean_ If createGroup is true, this determines if the group is collapased or not. Default: true.
  * **groupText** _string_ If create group is true, this will be the name of the group. Default: "Parameters"

## APEX Integration ##
The console wrapper was originally designed for [Oracle APEX](http://apex.oracle.com) applications. If implemented as part of an APEX application the documentation above is correct except for the following points:

  * _**Setup**_: You do not need to include the jQuery file as it is already included in APEX 4.0 and above
  * _**Levels**_: In APEX if you are in debug mode the console wrapper will default to the "log" level.


# Additional Resources #
  * Console APIs: http://getfirebug.com/wiki/index.php/Console_API
  * Example: http://getfirebug.com/logging
  * Example: http://www.tuttoaster.com/learning-javascript-and-dom-with-console/


# Change Log #
  * 1.0.4
    * Blogs:
      * http://www.talkapex.com/2013/04/console-wrapper-104.html
      * http://www.talkapex.com/2013/03/console-wrapper-104-beta.html
    * Fix issue with IE9/10 (i.e. versions greater than IE8 which displayed the "Object doesn't support property or method 'apply'
    * For console methods that aren't supported by IE (ex group) will default to .log
  * 1.0.3
    * In `logParams`, changed "log" to "warn" for unassigned parameters
  * 1.0.2
    * Fixed issue if first argument was not a string (error was occurring on first argument check)
  * 1.0.1
    * Added `logParams` function
    * Fixed `canLog` function (didn't detect the off level before)
  * [1.0.0](../..//releases/tag/1.0.0)
    * Initial