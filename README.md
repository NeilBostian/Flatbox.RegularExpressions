# Flatbox.RegularExpressions
A modified version of .NET Standard Regex which supports RegexOptions.Compiled

This is a merged repo between [.NET Framework Regex](https://github.com/Microsoft/referencesource/tree/master/System/regex/system/text/regularexpressions) and [.NET Standard Regex](https://github.com/dotnet/corefx/tree/master/src/System.Text.RegularExpressions/src/System/Text/RegularExpressions). All files within the CompiledRegularExpressions.csproj project come from these two repositories under MIT License. Porting your regex should be as easy as adding a reference to Flatbox.RegularExpressions and updating System.Text.RegularExpression namespace references.

### Why?
As seen in [various benchmarks](https://benchmarksgame.alioth.debian.org/u64q/regexredux.html), regex is unreasonably slow on .NET Standard. The reason for this is that compiling a regex at runtime requires System.Reflection.Emit. In .NET Framework this package was included with the runtime. With [.NET Native](https://docs.microsoft.com/en-us/dotnet/framework/net-native/), other packages in .NET Standard such as System.Text.RegularExpressions can't depend on System.Reflection.Emit or RegularExpressions wouldn't be able to run on .NET Native. The result is this package: a version of .NET Standard's [System.Text.RegularExpressions](https://github.com/dotnet/corefx/tree/master/src/System.Text.RegularExpressions/src/System/Text/RegularExpressions) which includes the compiled regex dependencies. [Read More](https://github.com/dotnet/corefx/issues/340)

