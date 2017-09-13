# Flatbox.RegularExpressions
A modified version of .NET Standard Regex which supports RegexOptions.Compiled

This is a merged repo between [.NET Framework Regex](https://github.com/Microsoft/referencesource/tree/master/System/regex/system/text/regularexpressions) and [.NET Standard Regex](https://github.com/dotnet/corefx/tree/master/src/System.Text.RegularExpressions/src/System/Text/RegularExpressions). All files within the CompiledRegularExpressions.csproj project come from these two repositories under MIT License. Porting your regex should be as easy as adding a reference to Flatbox.RegularExpressions and updating System.Text.RegularExpression namespace references.

### Install
View the [NuGet Page](https://www.nuget.org/packages/Flatbox.RegularExpressions) for the latest version.

### Why?
As seen in [various benchmarks](https://benchmarksgame.alioth.debian.org/u64q/regexredux.html), regex is unreasonably slow on .NET Standard. The reason for this is that compiling a regex at runtime requires System.Reflection.Emit. In .NET Framework this package was included with the runtime. With [.NET Native](https://docs.microsoft.com/en-us/dotnet/framework/net-native/), other packages in .NET Standard such as System.Text.RegularExpressions can't depend on System.Reflection.Emit or RegularExpressions wouldn't be able to run on .NET Native. The result is this package: a version of .NET Standard's [System.Text.RegularExpressions](https://github.com/dotnet/corefx/tree/master/src/System.Text.RegularExpressions/src/System/Text/RegularExpressions) which includes the compiled regex dependencies. [Read More](https://github.com/dotnet/corefx/issues/340)

### Benchmarks
In computationally heavy regex, Flatbox.RegularExpressions can provide 300%+ speed improvements. Consider the following code which evaluates [this well known regex](http://www.regular-expressions.info/catastrophic.html):

    using System;
    using System.Diagnostics;
    using Flatbox.RegularExpressions;

    public static class RegexBenchmark
    {
        public static void Main(string[] args)
        {
            const string testStr = "xxxxxxx";

            Stopwatch sw = new Stopwatch();
            Regex regex = new Regex("(x+x+)+y", RegexOptions.Compiled);
        
            sw.Start();
            for (int i = 0; i < 1000000; i++)
            {
                regex.IsMatch(testStr);
            }
            sw.Stop();
            Console.WriteLine("Elapsed time: " + sw.Elapsed);
        }
    }

Outputs:

    Elapsed time: 00:00:18.0565706

Changing from `using System.Text.RegularExpressions;` to `using Flatbox.RegularExpressions;` produces the following output:

    Elapsed time: 00:00:06.6457263

### To Do
* Create description strings for Exceptions
* Possibly remove System.Security.Permissions dependent code which could allow compilation for .NET Standard <2.0
