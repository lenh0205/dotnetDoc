> https://www.csharp.com/article/understanding-concurrency-in-c-sharp/

==================================================================================
# Concurrency
* -> is **doing more than one work at a given time** - while one operation is running, it should be possible for us to do another operation
* -> is crucial for improving **`responsiveness`** and **`performance`** in modern applications, especially those dealing with **I/O-bound operations or user interactions**

```cs - Ex: 
// while uploading to the server, we can get a response from the database
// while finishing the first request, respond to the second request.” That is what concurrency looks like
```

## CPU-bound and I/O-bound operations
* -> all these types of operations (CPU bound and I/O bound) together create the world of concurrency 
* -> it doesn't matter if it is CPU intensive or I/O relying operation, they are concurrency by their nature

## Best pratices
* -> Choose appropriate techniques based on operation type: Use parallelism for CPU-bound tasks and async/await for I/O-bound tasks.
* -> Avoid blocking the main thread: Use async/await or background threads to prevent UI responsiveness issues.
* -> Measure and profile: Use profiling tools to identify performance bottlenecks and determine the best optimization strategies.
* -> Understand the underlying hardware: Consider the number of CPU cores and I/O capabilities of the system when choosing techniques.
* -> Design for concurrency: Structure code with concurrency in mind to avoid race conditions and deadlocks.
* -> Use appropriate synchronization mechanisms: Protect shared resources with locks, mutexes, or other synchronization constructs

==================================================================================
# 'CPU-bound' operations

## Characteristics

Consume significant CPU resources for calculations and processing.
They spend most of their time actively using the CPU.
Typically involves complex algorithms, data processing, mathematical computations, or cryptography.
## Examples

Image processing
Data compression
Matrix computations
Scientific simulations
Password hashing
## Handling Techniques

Parallelism: Distribute work across multiple CPU cores using the Task Parallel Library (TPL) and techniques like Parallel.For and Parallel.ForEach.
Thread pools: Use thread pools to manage multiple threads efficiently, avoiding the overhead of creating and destroying threads for each operation.

```cs
class Program
{
    static void Main(string[] args)
    {
        Stopwatch stopwatch = Stopwatch.StartNew();

        // Simulate a CPU-intensive calculation using Parallel.ForEach
        Parallel.ForEach(Enumerable.Range(1, 100000), index =>
        {
            // Perform a computationally expensive task
            Math.Pow(index, index) * Math.Sin(index);
        });

        stopwatch.Stop();
        Console.WriteLine($"Total time elapsed: {stopwatch.ElapsedMilliseconds} milliseconds");
    }
}
```

# 'I/O-bound' operations

## Characteristics

Spend most of their time waiting for input/output operations to complete.
Involve interactions with external resources like databases, file systems, networks, or user input.
CPU usage is relatively low during these wait periods.

## Examples

Reading/writing files
Making network calls
Accessing databases
Handling user input

## Handling Techniques

Asynchronous programming: Use async and await keywords to enable non-blocking operations, allowing other code to execute while waiting for I/O.
Asynchronous I/O APIs: Leverage I/O-bound APIs that support asynchronous operations, such as HttpClient for network requests or FileStream for file I/O.

```cs
using System;
using System.Net.Http;
using System.Threading.Tasks;

namespace IOBoundExample
{
    class Program
    {
        static async Task Main(string[] args)
        {
            try
            {
                Stopwatch stopwatch = Stopwatch.StartNew();

                // Asynchronously download multiple files using HttpClient
                await Task.WhenAll(new[]
                {
                    DownloadFileAsync("https://example.com/file1.txt"),
                    DownloadFileAsync("https://example.com/file2.jpg"),
                    DownloadFileAsync("https://example.com/file3.pdf")
                });

                stopwatch.Stop();
                Console.WriteLine($"Total time elapsed: {stopwatch.ElapsedMilliseconds} milliseconds");
            }
            catch (Exception ex)
            {
                Console.WriteLine("Error occurred: {0}", ex.Message);
            }
        }

        static async Task DownloadFileAsync(string url)
        {
            using (HttpClient client = new HttpClient())
            {
                using (HttpResponseMessage response = await client.GetAsync(url))
                {
                    if (response.IsSuccessStatusCode)
                    {
                        using (Stream stream = await response.Content.ReadAsStreamAsync())
                        {
                            // Simulate file processing or saving
                            await ProcessFileAsync(stream);
                        }
                    }
                    else
                    {
                        Console.WriteLine($"Failed to download {url}: {response.StatusCode}");
                    }
                }
            }
        }

        static async Task ProcessFileAsync(Stream stream)
        {
            // Simulate file processing logic here
            await Task.Delay(500); // Simulate processing time
        }
    }
}
```
