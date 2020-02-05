This sample describes how to make `dotnet build` to build the .net framework projects

Recently I've got the problem for new solution.
There are two projects in the solution: 
* `ClassLibrary` is .net standard project used to define some helper classes. It was created in the `.net SDK` format
* `TestLibrary` is .net framework test project (since I'm going to use package in the .net framework application. It was created as old format

Both projects were created using the VS2019 16.4.4 using in-build templates. 

I've upgraded `TestLibrary` project reference from `package.config` to project references using in-built VS wizard 
(right click on `packages.config` file and there is an option like `migrate to package references`).

This option has converted `package.config` to the project references like
```xml
<ItemGroup>
  <PackageReference Include="MSTest.TestAdapter" Version="2.1.0" />
  <PackageReference Include="MSTest.TestFramework" Version="2.1.0" />                
</ItemGroup>
```

The problem is that `dotnet build` fails to build the `TestLibrary` project even after package reference conversion.

The error was 
> Class1.cs(50,10): error CS0246: The type or namespace name 'TestMethod' could not be found (are you missing a using directive or an assembly reference?) 

despite the fact that `MSTest.TestFramework` was successfully restored using `dotnet restore`.

You can try master branch to check this.

I have to change the project format to `.net sdk` to make `dotnet build` works with `TestLibrary` project.

There is newSdkProject branch available. 
The `TestLibrary` project has been converted to the new SDK format.

 