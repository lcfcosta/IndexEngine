![alt tag](https://github.com/jchristn/indexengine/blob/master/assets/icon.png)

# IndexEngine

[![][nuget-img]][nuget]

[nuget]:     https://www.nuget.org/packages/IndexEngine
[nuget-img]: https://badge.fury.io/nu/Object.svg

IndexEngine is a lightweight document and text indexing platform written in C# targeted to both .NET Core and .NET Framework.  IndexEngine uses Sqlite as a storage repository for index data. 

IndexEngine does NOT provide storage of the original documents.

## New in v2.0.0

- Breaking changes, major refactor
- Retarget from .NET 4.5.2 to .NET 4.6.1 (in addition to .NET Core)
- Migrate from unmanaged database layer to SqliteHelper
- Added support for IDisposable
- Database backup API
- Added index start and max results to search
- External logging via Logger
- Sync and async APIs for adding documents

## Help or feedback

First things first - do you need help or have feedback?  Contact me at joel dot christner at gmail dot com or file an issue here!

## Performance and scale

It's pretty quick :)  It hasn't been tested with large document libraries or large files, so I'd recommend testing thoroughly before using in production. 

## Simple Example
```
using Indexer;

IndexEngine ie = new IndexEngine("idx.db");

// Add a document
Document d = new Document(
  "GUID",              // GUID supplied by the caller
  "Title",             // i.e. Mark Twain
  "Description",       // i.e. A Great Book
  "File Path or URL",  // i.e. C:\Documents\MarkTwain.txt
  "Source",            // i.e. The Internet
  "AddedBy",           // i.e. Joel
  Encoding.UTF8.GetBytes("This is some sample data for indexing")
);
await ie.AddAsync(d);  // async
ie.Add(d);             // sync

// Search the index
List<string> terms = new List<string> { "some", "data" };
List<Document> results = ie.Search(terms);
foreach (Document d in results) Console.WriteLine(d.ToString());

// Find document GUIDs where terms can be found
List<string> guids = i.e.GetDocumentGuidsByTerms(terms);
foreach (string s in results) Console.WriteLine(s);

// List number of threads actively indexing documents
Console.WriteLine("Number of documents being indexed now: " + ie.CurrentIndexingThreads);

// Get the names of docs that are currently being indexed
IEnumerable<string> activeDocs = ie.DocumentsIndexing();

// Delete documents from the index
ie.DeleteDocumentByGuid("abcd1234...");
ie.DeleteDocumentByHandle("C:\\Documents\\MarkTwain.txt");

// Retrieve documents
Document d = GetDocumentByGuid("abcd1234...");
Document d = GetDocumentByHandle("C:\\Documents\\MarkTwain.txt");
bool b = IsGuidIndexed("abcd1234...");
bool b = IsHandleIndexed("C:\\Documents\\MarkTwain.txt");
```
