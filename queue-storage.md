```
dotnet add package WindowsAzure.Storage
```

### Send & Receive a message
```
using System;
using System.Threading.Tasks;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Queue;

namespace QueueApp
{
    class Program
    {

        private const string ConnectionString = "";

        static async Task Main(string[] args)
        {
            if (args.Length > 0)
            {
                string value = String.Join(" ", args);
                await SendArticleAsync(value);
                Console.WriteLine($"Sent: {value}");
            }
            else
            {
                string value = await ReceiveArticleAsync();
                Console.WriteLine($"Received {value}");
            }
        }

        static async Task SendArticleAsync(string newsMessage)
        {
            CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

            CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

            CloudQueue queue = queueClient.GetQueueReference("newsqueue");
            bool createdQueue = await queue.CreateIfNotExistsAsync();
            if (createdQueue)
            {
                Console.WriteLine("The queue of news articles was created.");
            }

            CloudQueueMessage articleMessage = new CloudQueueMessage(newsMessage);
            await queue.AddMessageAsync(articleMessage);
        }

        static CloudQueue GetQueue()
        {
            CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

            CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
            return queueClient.GetQueueReference("newsqueue");
        }
        
        static async Task<string> ReceiveArticleAsync()
        {
            CloudQueue queue = GetQueue();
            bool exists = await queue.ExistsAsync();
            if (exists)
            {
                CloudQueueMessage retrievedArticle = await queue.GetMessageAsync();
                if (retrievedArticle != null)
                {
                    string newsMessage = retrievedArticle.AsString;
                    await queue.DeleteMessageAsync(retrievedArticle);
                    return newsMessage;
                }
            }

            return "<queue empty or not created>";
        }
    }
}

```

### Crate, Receive, Delete
```
// Get the connection string from app settings
string connectionString = ConfigurationManager.AppSettings["StorageConnectionString"];

// Instantiate a QueueClient which will be used to create and manipulate the queue
QueueClient queueClient = new QueueClient(connectionString, "myqueue");

// Create the queue if it doesn't already exist
await queueClient.CreateIfNotExistsAsync();

if (await queueClient.ExistsAsync())
{
    Console.WriteLine($"Queue '{queueClient.Name}' created");
}
else
{
    Console.WriteLine($"Queue '{queueClient.Name}' exists");
}

// Async enqueue the message
await queueClient.SendMessageAsync("Hello, World");
Console.WriteLine($"Message added");

// Async receive the message
QueueMessage[] retrievedMessage = await queueClient.ReceiveMessagesAsync();
Console.WriteLine($"Retrieved message with content '{retrievedMessage[0].MessageText}'");

// Async delete the message
await queueClient.DeleteMessageAsync(retrievedMessage[0].MessageId, retrievedMessage[0].PopReceipt);
Console.WriteLine($"Deleted message: '{retrievedMessage[0].MessageText}'");

// Async delete the queue
await queueClient.DeleteAsync();
Console.WriteLine($"Deleted queue: '{queueClient.Name}'");
```

#### De queue
```
// Get the connection string from app settings
string connectionString = ConfigurationManager.AppSettings["StorageConnectionString"];

// Instantiate a QueueClient which will be used to create and manipulate the queue
QueueClient queueClient = new QueueClient(connectionString, "myqueue");

if (queueClient.Exists())
{
    // Receive and process 20 messages
    QueueMessage[] receivedMessages = queueClient.ReceiveMessages(20, TimeSpan.FromMinutes(5));

    foreach (QueueMessage message in receivedMessages)
    {
        // Process (i.e. print) the messages in less than 5 minutes
        Console.WriteLine($"De-queued message: '{message.MessageText}'");

        // Delete the message
        queueClient.DeleteMessage(message.MessageId, message.PopReceipt);
    }
}
```

### Peeek
```
// Get the connection string from app settings
string connectionString = ConfigurationManager.AppSettings["StorageConnectionString"];

// Instantiate a QueueClient which will be used to create and manipulate the queue
QueueClient queueClient = new QueueClient(connectionString, "myqueue");

if (queueClient.Exists())
{ 
    // Peek at the next message
    PeekedMessage[] peekedMessage = queueClient.PeekMessages();

    // Display the message
    Console.WriteLine($"Peeked message: '{peekedMessage[0].MessageText}'");
}
```

### Update
```
// Get the connection string from app settings
string connectionString = ConfigurationManager.AppSettings["StorageConnectionString"];

// Instantiate a QueueClient which will be used to create and manipulate the queue
QueueClient queueClient = new QueueClient(connectionString, "myqueue");

if (queueClient.Exists())
{
    // Get the message from the queue
    QueueMessage[] message = queueClient.ReceiveMessages();

    // Update the message contents
    queueClient.UpdateMessage(message[0].MessageId, 
            message[0].PopReceipt, 
            "Updated contents",
            TimeSpan.FromSeconds(60.0)  // Make it invisible for another 60 seconds
        );
}
```

### Get the queue length
```
// Get the connection string from app settings
string connectionString = ConfigurationManager.AppSettings["StorageConnectionString"];

// Instantiate a QueueClient which will be used to create and manipulate the queue
QueueClient queueClient = new QueueClient(connectionString, "myqueue");

if (queueClient.Exists())
{
    QueueProperties properties = queueClient.GetProperties();

    // Retrieve the cached approximate message count.
    int cachedMessagesCount = properties.ApproximateMessagesCount;

    // Display number of messages.
    Console.WriteLine($"Number of messages in queue: {cachedMessagesCount}");
}
```

### Delete a queue
```
// Get the connection string from app settings
string connectionString = ConfigurationManager.AppSettings["StorageConnectionString"];

// Instantiate a QueueClient which will be used to create and manipulate the queue
QueueClient queueClient = new QueueClient(connectionString, "myqueue");

if (queueClient.Exists())
{
    // Delete the queue
    queueClient.Delete();
}
```
