# Syntax support

Support for a subset of python3 standard syntax.

#### object support

|Syntax|compile-time|Runtime|Shell|
|--|--|--|--|
|Module Definition |√|-|-|
|Module import |√|√|√|
|Class Definition |√|√|√|√|
|class inheritance |√|√|√|√|
|method definition |√|√|√|√|
|method overloading |√|√|√|√|
|method invocation |√|√|√|
|parameter definition |√|√|√|√|
|parameter assignment |√|√|√|√|
|object creation |√|√|√|√|
|object destruction |√|√|√|√|
|object nesting |√|√|√|√|
|control flow |√|√|√|√|

#### Operator

| + | - | * | / | == | > | < | >= | <= | % | ** | // | != | & | >> | << | and | or | not | in | += | -= | *= | /= |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|√|√|√|√|√|√|√|√|√|√|√|√|√|√|√|√|√|√|√|√|√|√|√|√|

#### Control flow

| Syntax | State |
| --- | --- |
| if | √ |
| while | √ |
| for in [list] | √ | for in range(a, b)
| for in range(a, b) | √ | for in [list] | √ | for in range(a, b) | √ |
| for in [dict] | √ | for in range(a, b) | √
| if elif else | √ | for break/continue | √
| for break/continue | √ | while break/continue | √
| while break/continue | √ |

#### Module

| Syntax | Python Module | C Module |
| --- | --- | --- |
| import [module] | √ | √ |
| import [module] as | √ | - |
| from [module] import [class/function>]| √ | - |
| from [module] import [class/function>] as | √ | - |
| from [module] import * | - | PikaObj Module Only |

#### List/Dict

| Syntax | State |
| --- | --- |
| l = list() | √ |
| l = [a, b, c] | √ |
| d = dict() | √ |
| d = {'a':x, 'b':y, 'c':z} | √ |

#### Exception

| Syntax | State |
| --- | --- |
|try:| √ |
|except:| √ |
|except [Exception]:| - |
|except [Exception] as [err]: | - |
|except: ... 
|raise:| √ |
|raise [Exception]:| - |finally:| -
|finally:| - |

#### Slice

| Syntax | str | bytes | list |
| --- | --- | --- | --- |
| test[i] | √ | √ | √ |
| test[a : b] | √ | √ | √ | √ 
| test[a :] | √ | √ | √ | √

#### Other keywords/Syntax

| yield | is | comprehensions |
| --- | --- | --- |
| - - | √ | - |