# Get started with ASP.NET Core SignalR

# Description
Build an ASP.NET Core SignalR app. 
The app will be built with the following steps:
1. Create a web project.
2. Add the SignalR client library.
3. Create a SignalR hub.
4. Configure the project to use SignalR.
5. Add code that sends messages from any client to all connected clients.

# Create a web app project
```
dotnet new webapp -o SignalRChat
code -r SignalRChat
```

# Add the SignalR client library
```
dotnet tool uninstall -g Microsoft.Web.LibraryManager.Cli
dotnet tool install -g Microsoft.Web.LibraryManager.Cli
```
Navigate to the project folder, which contains the SignalRChat.csproj file.

Run the following command to get the SignalR client library by using LibMan. It may take a few seconds before displaying output.

```
libman install @microsoft/signalr@latest -p unpkg -d wwwroot/js/signalr --files dist/browser/signalr.js --files dist/browser/signalr.js
```

# Create a SignalR hub
A hub is a class that serves as a high-level pipeline that handles client-server communication.

In the SignalRChat project folder, create a Hubs folder.

In the Hubs folder, create the ChatHub class with the following code:
```
using Microsoft.AspNetCore.SignalR;

namespace SignalRChat.Hubs
{
    public class ChatHub : Hub
    {
        public async Task SendMessage(string user, string message)
        {
            await Clients.All.SendAsync("ReceiveMessage", user, message);
        }
    }
}
```

The ```ChatHub``` class inherits from the SignalR ```Hub``` class. The Hub class manages connections, groups, and messaging.

The ```SendMessage``` method can be called by a connected client to send a message to all clients. JavaScript client code that calls the method is shown later in the tutorial. SignalR code is asynchronous to provide maximum scalability.

# Configure SignalR
The SignalR server must be configured to pass SignalR requests to SignalR.
Add the following code to the ```Program.cs``` file.

```
using SignalRChat.Hubs;
```
```
builder.Services.AddSignalR();
```
```
app.MapHub<ChatHub>("/chatHub");
```

# Resources
To learn more about SignalR, see [Tutorial: Get started with ASP.NET Core SignalR](https://learn.microsoft.com/en-us/aspnet/core/tutorials/signalr?view=aspnetcore-7.0&tabs=visual-studio-code)
