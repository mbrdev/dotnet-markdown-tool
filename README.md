# .NET Markdown Tool

This .NET global tool provides functionalty to automatically inject code samples into your markdown files.

## Preparing your markdown files

To include the entire contents of a code file inside a code block, add a comment with a relative path to the file inside the code block;

````
```csharp
// source: src/project/Program.cs
```
````

Once the .NET Markdown Tool runs it will replace any content below that comment with the contents of the file;

````
```csharp
// source: src/project/Program.cs
HelloWorld();

public void HelloWorld()
{
    Console.WriteLine("Hello World!");
}

public void Goodbye()
{
    Console.WriteLine("Goodbye World!");
}
```
````

For C# source files you can add a `member:` property to the codeblock comment, this can be a class name or method name defined in the file and it will attempt to insert only that member, this is useful if you have files containing multiple classes or you want to show only a single method of a class and don't want to rely on line numbers which are likely to change over time.

````
```csharp 
// source: src/project/Program.cs member: HelloWorld
public void HelloWorld()
{
    Console.WriteLine("Hello World!");
}
```
````

You can use the `lines:` property in the codeblock comment to pull in a specific range of lines from the file.

````
```csharp 
// source: src/project/Program.cs lines: 5-6
    Console.WriteLine("Hello World!");
}
```
````

The source property should always be on the first line of the codeblock and should start with a comment marker using any of the following (which ever comment style best matches the language of the code block);

- `//`
- `#`
- `<!--`
- `--`
- `/*`

The comment marker will tell the tool to check for the `source:` property and any other properties on that line. You can use comment closing markers as well such as in XML comments;

````
```xml
<!-- source: src/project/Project.csproj -->
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net8.0</TargetFramework>
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>enable</Nullable>
  </PropertyGroup>

</Project>
```
````

The tool will replace everything within the codeblock (it will search for the next ````` match) except the first line with the specified file/content so it can be run multiple times to update existing codeblocks with updated content.

## Installing the tool

To install the tool globally use the following command;

```sh
dotnet tool install --global MbrDev.MarkdownTool
```

To install the tool for a specific repository you can replace the `--global` `--local`.

```sh
dotnet tool install --local MbrDev.MarkdownTool
```

Note: Installing it locally requires a local tool manifest file. If you do not already have one you can create it with the below command;

```sh
dotnet new tool-manifest
```

## Using the tool

### Update Sub-Command

To update a single markdown file just provide the relative or absolute file path as the final argument.

```sh
dotnet markdown update README.md
```

To update any markdown file in the current directory use `.` as the final argument.

```sh
dotnet markdown update .
```

To use the current directory and all subdirectories, use the `--recurse` option. This option can only be used when the target is a directory.

```sh
dotnet markdown update --recurse .
```

To filter markdown files use the `--filter` option which works in the same way as the `searchPattern` variable in .NET's `Directory.GetFiles("folder", "*.csv")` method and as such only supports `*` and `?` wildcards.

```sh
dotnet markdown update --filter *example*.md .
```

### Scan Sub-Command

If you want to scan for markdown files containing codeblocks that are not annotated with source comments, use the `scan` sub-command, the same options for the `update` work in the `scan` sub-command. This is very useful for tracking down codeblock examples that are not yet being updated automatically. It will also detail any issues such as missing files or invalid options where a source annotation is recognized. No changes will be made to any files when using the `scan` sub-command.

```sh
dotnet markdown scan --recurse .
```