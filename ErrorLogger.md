# Error Logger

This class consists of functions that write error messages to text files for you to read. It also lets you write logs to text files.

## Fields

### public static readonly string LogPath

The file path to which logs are written and stored.

## Methods

### public static void Log(string message)

You can use this method for your own testing purposes. The message will be added to the Logs.txt file in the Logs folder.

### public static void ClearLog()

Deletes all text in the Logs.txt file.