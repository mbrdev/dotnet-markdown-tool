# .NET Markdown Tool

This .NET global tool provides functionalty to automatically inject code samples into your markdown files.

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
```
````

For `.cs` source files you can add a `member:` property to the codeblock comment, this could be a class, field, property or method defined in the file and it will attempt to insert only that member.

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





<!-- 
actionscript3
apache
applescript
asp
brainfuck
c
cfm
clojure
cmake
coffee-script, coffeescript, coffee
cpp – C++
cs
csharp
css
csv
bash
diff
elixir
erb – HTML + Embedded Ruby
go
haml
http
java
javascript
json
jsx
less
lolcode
make – Makefile
markdown
matlab
nginx
objectivec
pascal
PHP
Perl
python
profile – python profiler output
rust
salt, saltstate – Salt
shell, sh, zsh, bash – Shell scripting
scss
sql
svg
swift
rb, jruby, ruby – Ruby
smalltalk
vim, viml – Vim Script
volt
vhdl
vue
xml – XML and also used for HTML with inline CSS and Javascript
yaml 
-->