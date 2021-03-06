### Welcome to GGLog
This project is a very simple swift logging kit. It is designed to replace `print` and give you the extra power of adding tags to your logs. Check out the examples below to see how useful this can be!

### To use
To use GGLog in your own project simply download the `GGLog.swift` file and include it in your project. That's it, you're good to go!

### Most simple use
Just a simple print to the console.

    GGLog.log("This is a log!")
    => GGLog: This is a log!

This method will accept any object that conforms to the `CustomStringConvertible` (previously `Printable`) protocol and log it to the console.

### Simple tags
Print an object with a specified tag.

    GGLog.log("This is a log, with a custom tag!", tag: "MyLogs")
    => MyLogs: This is a log, with a custom tag!

When printed to the console, the log will be prefixed with the tag, this makes it nice and simple to search for specific logs in the console.

### The power of tags
Remember all those times you've hit an issue with your code, added a few logs to see what's going on, and then had the nightmare of trying to find these new logs in amongst all the others that you have in your project? Perhaps not, but if you have... Check this out:

So we have all these logs in the app already, and perhaps quite a few more:

    GGLog.log("App launched")
    GGLog.log("App is online!")
    GGLog.log("App did something good!")

And then we add a new log to help debug this issue that we're having:

    GGLog.log("Hmmm, went down this code path...", tag: "Debug")
    => GGLog: App launched
    => GGLog: App is online!
    => GGLog: App did something good!
    => Debug: Hmmm, went down this code path...

And it's hard to find what we want in all of these logs, well not in this case, but imagine that there are loads more (if your code's anything like mine, it won't take much imagining :P). All you need to do, is pop this line:

    GGLog.soloTags(["Debug"])

into your `application:didFinishLaunchingWithOptions:`, and all of a sudden the log output of your app is:

    => Debug: Hmmm, went down this code path...

You can very easily turn off all logs in your app except for the ones that are relevant to what you are debugging right now! Make sure you remember to configure the logger before you start making calls to log, only the calls to log after the config calls will be affected... obviously :P

### Changing the status of a tag
GGLog lets you enable or disable tags in your project. Simply make a call to `setTag:enabled:` like so:

    GGLog.setTag("Debug", enabled: false)

to change the state of a specific tag. If false is passed the logger will ignore any future calls to log with the specified tag, and if true is passed a tag will be re-enabled. By default all tags are enabled and will be printed.

### Solo tags

So as we saw before you can use the `soloTags:` method as follows:

    GGLog.soloTags(["Debug"])

This takes a list of tags, and simply tells the logger that it should only print logs with the tags specified in this call. It basically disables all logs with a tag that is not in this list.

### Muting the logger
If you want to stop your app from logging altogether, perhaps when you release your app, simply call the mute method as follows:

    GGLog.mute()

and the logger will ignore calls to log.

### Log schemes
The GGLog kit provides one additional class to help configure your logs, `GGLogScheme`. A log scheme object can be created with one of the following class methods:

    class func disabledTagsScheme(disabledTags: [String]) -> GGLogScheme
    class func soloTagsScheme(soloTags: [String]) -> GGLogScheme
    class func muteScheme() -> GGLogScheme

You may notice the similarity between these methods and the sections above. You can then load a log scheme into the logger like this:

    let scheme = GGLogScheme.soloTagsScheme(["Debug"])
    GGLog.loadLogScheme(scheme)

and the logger will be configured, in this case it will now only print calls to log made for the tag "Debug". You can use this to, for example, create a log scheme for each of your build schemes so that when you do a debug/release build the logger automatically picks up the relevant scheme. Also, just switching between different sets of logs when debugging can be quite useful.

### More info
For more info, see the documentation comments in the GGLog.swift file, the source code itself, or checkout [my site](http://georgegreen.london/gglog). You can for example, change the default tag used when using the simple log method, log to standard error (not much use for iOS) instead of standard out, or even log to your own custom NSFileHandle.

### Any queries?
If you have any troubles with GGLogs, or feature requests, feel free to leave a comment or send me an [email](mailto:gglog@georgegreen.london).
