# Introductory
Starting with tModLoader releases _above version 0.10.1.5_, the API uses the [log4net library](https://logging.apache.org/log4net/) to facilitate logging functionality to modders.

# Prerequisites
1. To use this logging in your mod, you must have a release _above version 0.10.1.5_.
1. If you use an IDE (such as VS), you must also have a reference to the log4net.dll file.
   1. It is recommended to take the one tML uses, located in the [/references](https://github.com/tModLoader/tModLoader/tree/master/references) path.
   1. As per usual, if you want to put this reference inside your mod source directory then you should put it inside your /lib path, and ensure /lib is ignored during the build process.

# Improvements
The logging updates build on the old logging, which was mediocre and had nearly no control, and provides many new functionalities and solves a few issues we had. 

## Detailed logs
Logs are now split between files, e.g: client.log and server.log
Every log output provides at least generic log info: time, loglevel and source.
![](https://i.imgur.com/SVzuUE5.png)

## Archiving
Old logs are archived in a compressed format so they can be revisited at a later date. 
![](https://i.imgur.com/Oos1nxm.png)

# Usage
Provided in your mod comes your own Logger object. (`myMod.Logger`) You can use this to log things on behalf of your mod. Inside the log file, your mod will be marked as said source of the log.

There are a few different levels of logging:

**INFO** : Should be used for generic informational messages <br/>
**WARN** : Should be used as a warning, for example when something went wrong but didn't cause critical problems <br/>
**ERROR** : Should be used for critical problems, such as a mod breaking, potential crashing or malfunction of systems <br/>
**FATAL**: Should be used when the application can no longer do useful work due to a critical problem <br/>
**DEBUG** : Should be used for additional debugging information (verbose) <br/>

Knowing the difference between error and fatal logging can be difficult, but generally you can follow this rule of thumb: \`\`an error is fatal if the application can simply not continue due to the problem causing the error´´

Examples:
```cs
mod.Logger.Info("This is an informational log"); // at log-level INFO
mod.Logger.InfoFormat("This is a formatted informational log from {0}", mod.Name);

mod.Logger.Warn("This is a warning"); // at log-level WARN
mod.Logger.WarnFormat("This is a warning from {0}", mod.Name);

mod.Logger.Error("An error occurred"); // at log-level ERROR
mod.Logger.ErrorFormat("An error occurred: {0}", e.StackTrace);

mod.Logger.Fatal("Something fatal happened"); // at log-level FATAL
mod.Logger.FatalFormat("Something fatal happened: {0}", e.StackTrace);

mod.Logger.Debug("Some debug info"); // at log-level DEBUG
mod.Logger.DebugFormat("Some debug info: ({0}, {1})", x, y);
```

# What should I log?
It can be hard to determine what should be logged. In general, you should try to avoid bloating log files with useless items. Ask yourself: \`\`what purpose does this item have if logged?´´. You want your logs to be useful, so they need a _purpose_. The purpose of a log is usually to identify issues (or to ensure there are none!) Ideally, you only log items that are required for this purpose. For example, you should push an informational log if a system in your mod completed a particular task successfully (e.g. setting up). In case it failed, it would push a warning or error instead. This way, when inspecting logs, you could identify an issue with the initialization of this system if it occurred.