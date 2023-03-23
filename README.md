# NuPack

An app to help learn about publishing Nuget packages to Github's package manager.

## Steps

These are the steps I used to create the repository content and publish the package. The package was uploaded the packages under my user account and not a specific project.

### Setup GitHub project

Create a new repository on GitHub called "NuPack".

Locally from a command prompt run the following commands.

```cmd
git init NuPack

cd NuPack

echo # NuPack >> README.md

git checkout -b main

git commit -m "initial project with readme"

git remote add origin https://github.com/jswanso/NuPack.git
```

### Available DotNet Commands

```dotnet new --list```

### Add a gitignore File

```dotnet new gitignore```

### Create .NET Project

```dotnet new classlib --name AppDev.Numbers```

After the project was created I renamed the class and added some content to it.

### Commit Changes to GitHub.com

```cmd
git add .
git commit -m "Initial class"
git push -u origin main
```

### Create Package

```dotnet pack --configuration Release AppDev.Numbers```

This creates a NuGet package located as follows: AppDev.Numbers\bin\Release\AppDev.Numbers.1.0.0.nupkg. if you want to set the version you can use the following command.

```dotnet pack --configuration Release /p:Version=1.0.1 AppDev.Numbers```

See [stackoverflow dotnet pack version](https://stackoverflow.com/questions/42797993/package-version-is-always-1-0-0-with-dotnet-pack) for some details on the version setting.

### Register a new NuGet Source

Commands to troubleshoot NuGet settings.

```cmd
dotnet nuget --help
dotnet nuget list 
dotnet nuget list client-cert
dotnet nuget list source
```

See the following [dotnet nuget list source](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-nuget-list-source) and
[common NuGet configurations](https://learn.microsoft.com/en-us/nuget/consume-packages/configuring-nuget-behavior) documentation.

The next command adds a "github" source to your NuGet.Config located C:\Users\<USER NAME>\AppData\Roaming\NuGet\NuGet.Config.

```dotnet nuget add source --username jswanso --password <PERSON ACCESS TOKEN> --store-password-in-clear-text --name github "https://nuget.pkg.github.com/jswanso/index.json"```

Note you can remove the source you just added with the following:

```dotnet nuget remove source github```

You can also just register the source without storing user information, but then you need the api-key when you publish.

```dotnet nuget add source --name github "https://nuget.pkg.github.com/jswanso/index.json"```

### Publish Package

```dotnet nuget push "AppDev.Numbers/bin/Release/AppDev.Numbers.1.0.0.nupkg" --api-key <PERSON ACCESS TOKEN> --source "github"```

If the credentials are saved in the NuGet.Config file then you can push without the api-key value set, but the command line did throw a warning.

```dotnet nuget push "AppDev.Numbers/bin/Release/AppDev.Numbers.1.0.0.nupkg" --source "github"```

### Consuming the Package

This assumes you have the credentials to the source saved in your NuGet.config file.

```dotnet add package AppDev.Numbers --version 1.0.0 -s https://nuget.pkg.github.com/jswanso/index.json```
