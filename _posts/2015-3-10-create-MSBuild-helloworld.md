##从头创建MSBuild项目文件

####. 创建一个C#文件：
```
	using System;

		class HelloWorld
	{
	    static void Main()
	    {
	#if DebugConfig
	        Console.WriteLine("WE ARE IN THE DEBUG CONFIGURATION");
	#endif
	
	        Console.WriteLine("Hello, world!");
	    }
	}
```

可使用csc命令直接编译成exe文件。
>注：csc命令要添加到系统path路径

```
	csc helloworld.cs
```

####创建最小的MSBuild项目文件
最小的MSBuild项目文件包括：
 - 必需的根 Project 节点。
- 用于包含项元素的 ItemGroup 节点。
- 引用应用程序源文件的项元素。
- 一个 Target 节点，用于包含生成应用程序所需的任务。
- 一个 Task 元素，用于启动 Visual C# 编译器以生成应用程序。

MSBuild代码如下：

```
	<Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
		<ItemGroup>
			<Compile Include="helloworld.cs" />
		</ItemGroup>
		<Target Name="Build">
			<Csc Sources="@(Compile)"/>
		</Target>
	</Project>
```

保存文件为test.csproj

解释一下以上文件：

MSBuild依次执行文件中的任务，在本例子中编译是唯一任务，编译任务去编译一系列代码，这一系列代码由Compile项的值指定。目前Compile项只有一个源文件，helloworld.cs

使用以下命令行即可编译工程，输出exe文件。

```
	MsBuild test.csproj
```

####添加生成属性
将生成属性添加到项目中，包括：
- AssemblyName 属性，用于指定应用程序的名称
- OutputPath 属性，用于指定要包含应用程序的文件夹。

添加属性后代码如下：

```
<Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
	<PropertyGroup>
		<AssemblyName>MSBuildSample</AssemblyName>
		<OutputPath>Bin\</OutputPath>
	</PropertyGroup>
	<ItemGroup>
		<Compile Include="helloworld.cs" />
	</ItemGroup>
	<Target Name="Build">
		<MakeDir Directories="$(OutputPath)"      Condition="!Exists('$(OutputPath)')" />
		<Csc Sources="@(Compile)" OutputAssembly="$(OutputPath)$(AssemblyName).exe" />
	</Target>
</Project>
```

解释一下以上代码：

PropertyGroup元素的子元素说明定义了哪些属性，这里定义了AssemblyName和OutputPath。

在编译任务之前，首先执行创建文件夹的任务，前提是该文件夹不存在。

编译任务中添加一个输出路径的说明，使用前面定义的两个属性。


执行代码以后生成.\Bin\MSBuildSample.exe


####添加生成目标
可以向项目文件中增加目标，本例子增加两个目标:
- Clean目标,删除旧文件
- Rebuild目标，该目标使用DependsOnTargets 特性，保证Clean人物在BUild任务之前运行。

添加了目标以后，要设置一个默认目标，这里将Build目标设置为默认目标。

添加以上两目标后的代码如下：

```
<Project DefaultTargets="Build" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
	<PropertyGroup>
		<AssemblyName>MSBuildSample</AssemblyName>
		<OutputPath>Bin\</OutputPath>
	</PropertyGroup>
	<ItemGroup>
		<Compile Include="helloworld.cs" />
	</ItemGroup>
	<Target Name="Build">
		<MakeDir Directories="$(OutputPath)" Condition="!Exists('$(OutputPath)')" />
		<Csc Sources="@(Compile)" OutputAssembly="$(OutputPath)$(AssemblyName).exe" />
	</Target>
	<Target Name="Clean" >
		<Delete Files="$(OutputPath)$(AssemblyName).exe" />
	</Target>
	<Target Name="Rebuild" DependsOnTargets="Clean;Build" />
</Project>
```

解释：
Clean目标删除已有的应用程序，Rebuild目标没有实际的任务，但是它保证了Clean目标在Build目标之前运行。

**msbuild命令**

使用命令行 `msbuild helloworld.csproj /p:AssemblyName=Greetings`可以build工程文件的设置属性`AssemblyName`。

使用命令行`msbuild helloworld.csproj /t:clean `可执行clean目标，使用`/t:`指定target，不指定的话使用默认目标。


####增量方式生成
增量方式生成的意思是尽在目标所依赖的源文件或者目标文件更改时才重新生成目标，MSBuild使用文件的时间戳确定文件是否更改。

使用增量方式生成的代码如下：

```
<Project DefaultTargets="Build" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
	<PropertyGroup>
		<AssemblyName>MSBuildSample</AssemblyName>
		<OutputPath>Bin\</OutputPath>
	</PropertyGroup>
	<ItemGroup>
		<Compile Include="helloworld.cs" />
	</ItemGroup>
	<Target Name="Build" Inputs="@(Compile)" Outputs="$(OutputPath)$(AssemblyName).exe">
		<MakeDir Directories="$(OutputPath)" Condition="!Exists('$(OutputPath)')" />
		<Csc Sources="@(Compile)" OutputAssembly="$(OutputPath)$(AssemblyName).exe" />
	</Target>
	<Target Name="Clean" >
		<Delete Files="$(OutputPath)$(AssemblyName).exe" />
	</Target>
	<Target Name="Rebuild" DependsOnTargets="Clean;Build" />
</Project>
```

其中Build目标增加了属性`Inputs="@(Compile)" Outputs="$(OutputPath)$(AssemblyName).exe"`，制定Build目标依赖于Compile项目中指定的输入文件，切输出目标为程序文件。


####关于target的解释
target类似于你设置了几种build目标，在使用MSBuild时，告知其目标是什么。

比如这里的build、clean以及rebuild，也可以是release或debug。

目标中可能包括一系列任务或者子target，比如例子中Build包括创建目录和编译。Rebuild又包括clean和build


代码：http://pan.baidu.com/s/1eQcY38Q