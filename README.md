# NetEscapades.EnumGenerators
A greatly simpflied version of [Andrew Lock's enum generators](https://github.com/andrewlock/NetEscapades.EnumGenerators)
which is the source code of his famous blog series [Series: Creating a source generator](https://andrewlock.net/series/creating-a-source-generator/). 

This can be used to reproduce a SonarQube / Sonarscanner bug that breaks the build when using source generators.

When building this with the usual `dotnet build` everything compiles just fine.
But when I run `begin` of the sonar scanner first,

> dotnet C:\Users\bitbonk.nuget\packages\dotnet-sonarscanner\5.5.3\tools\net5.0\any\SonarScanner.MSBuild.dll begin /k:my.key /n:"My Title" /v:1.1.0-sonarqube-sourcegenerator.1 /d:sonar.host.url=https://my.website.com /d:sonar.login=MyLogin /d:sonar.password=MyPassword /d:sonar.coverageReportPaths=c:\source\repos\github\bitbonk\NetEscapades.EnumGenerators\artifacts\test-results\sonarqube\SonarQube.xml /d:sonar.links.homepage=https://my.selfhosted.sonarqube.com/Foo/Bar/_git/NetEscapades.EnumGenerators

a subsequent `dotnet build` will fail.

> C:\source\repos\github\bitbonk\NetEscapades.EnumGenerators\tests\NetEscapades.EnumGenerators.IntegrationTests\EnumWithSameDisplayNameExtensionsTests.cs(32,84): error CS1061: 'EnumWithSameDisplayName' does not contain a definition for 'ToStringFast' and no accessible extension method 'ToStringFast' accepting a first argument of type 'EnumWithSameDisplayName' could be found (are you missing a using directive or an assembly reference?) [C:\source\repos\github\bitbonk\NetEscapades.EnumGenerators\tests\NetEscapades.EnumGenerators.IntegrationTests\NetEscapades.EnumGenerators.IntegrationTests.csproj]

None of the source generated types can be found in the consuming integration project.

The source generated stuff is referenced in the integration test like so:

```csproj
  <ItemGroup>
    <ProjectReference Include="..\..\src\NetEscapades.EnumGenerators\NetEscapades.EnumGenerators.csproj"
                      OutputItemType="Analyzer"
                      ReferenceOutputAssembly="false" />
    <ProjectReference Include="..\..\src\NetEscapades.EnumGenerators.Attributes\NetEscapades.EnumGenerators.Attributes.csproj"
                      OutputItemType="Analyzer"
                      ReferenceOutputAssembly="true" />
  </ItemGroup>
```
