# ModPacket

This class inherits from [BinaryWriter](https://msdn.microsoft.com/en-us/library/system.io.binarywriter(v=vs.110).aspx). This means that you can use all of its writing functions to send information between client and server. This class also comes with a Send method that's used to actually send everything you've written between client and server.

## Methods

ModPacket has all the same methods as BinaryWriter. In addition, it has:

### public void Send(int toClient = -1, int ignoreClient = -1)

Sends all the information you've written between client and server. If the toClient parameter is non-negative, this packet will only be sent to the specified client. If the ignoreClient parameter is non-negative, this packet will not be sent to the specified client.