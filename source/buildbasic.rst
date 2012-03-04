.. highlight:: rst

.. _buildBasics:

构建脚本基础
============================

项目(Projects)和任务(tasks)
------------------------------------------------------------    
Gradle 中的任何内容都是基于项目和任务这两个基本概念。

每个Gradle构建都是由一个或多个项目组成。一个项目代表你软件产品中可以独立构建的一些模块。具体的依赖于你要构建什么。例如，一个项目可以代表一个jar包或者是一个web应用程序。他也可以代表将一堆由其他项目提供的jar组装成一个发布zip。项目不一定代表构建什么东西，也可以代表一些动作比如发布你应用到测试或生产环境。如果现在理解这些有点模糊也没有关系。

每个项目都是由一个或多个任务组成。每个任务代表构建系统中的一个原子操作。例如编译一些类，创建一个jar，生成Java 文档或者发布一些包到仓库中。

现在我们就来看看在一个项目中定义一些简单的任务。在后续的章节中我们将介绍（） 以及关于项目和任务的更多内容。

Hello world
------------------------------------------------------------
你可以通过 ``gradle`` 命令来运行一个Gradle 构建。 ``gradle`` 命令将会在当前目录中寻找名为 ``build.gradle`` 的文件 [2]_ 。从严格意义上来讲 ``build.gradle`` 是一个构建配置脚本，但我们还是习惯性的称他为构建脚本。 这个构建脚本定义了一个项目（project) 以及这个项目的任务。

动手尝试，创建一个名为 build.gradle 的文件，其内容如下。

.. code-block:: groovy 

    task hello {
        doLast {
            println 'Hello world!'
        }
    }
    
在命令行中，进入build.gradle 所在目录并通过 ``gradle -q hello`` 来运行这个脚本。

.. sidebar:: -q的作用

   在该文档中大部分例子都会带一个 -q 选项。阻止Gradle本身log输出。这样就只有任务的输出才会显示。这样就能让该手册中的例子的输出清晰一些。你也可以不适用这个选项。要获得更多关于Gradle关于输出的选项请参考 第16章 ::ref:logging

**gradle -q hello** 的输出结果如下

.. code-block:: groovy

        > gradle -q hello
        Hello world!

这是怎么回事?这个构建脚本定义了一个叫做hello的任务，并给他加上了一个动作。当运行 **gradle hello** 时，Gradle就执行了这个任务。这个动作其实就是包含了一些Groovy代码的闭包。
如果你觉得他和Ant中的target有点类似的话，那就对了。 Gradle的任务（task）就等价于Ant的target。但他要强大的多，这一点你马上可以看到。we have usedd a different terminology than ant as we think the word task is more expressive than the word target. (这里是接受表达)。但不幸的是这种叫法和Ant的命令冲突，他把如 javac 或 copy 这样的命令为任务（task）。所以我们说到的任务（task），都是指等价于Ant中的target的那个Gradle 任务。如果是指Ant的任务时，我们会称它会Ant任务（Ant task）。

短任务的定义
------------------------------------------------------------

和上面定义hello任务相比，还有一种更优雅的方式来定义任务。
build.gradle

.. code-block:: groovy

    task hello << {
        println 'Hello world!'
    }

和上面一样，这里的定义了一个只有一个闭包，叫hello的任务。 我们将在整个文档中使用这种风格来定义任务。


构建脚本就是代码
------------------------------------------------------------   

通过Gradle 构建脚本你可以了解到Groovy的强大的能力。 先来一个开胃小菜:

.. code-block:: groovy
   
    task upper << {
        String someString = 'mY_nAmE'
        println "Original: " + someString
        println "Upper case: " + someString.toUpperCase()
    }

:strong:`gradle -q upper` 的输出

.. code-block:: groovy

    > gradle -q upper
    Original: mY_nAmE
    Upper case: MY_NAME
    
或者


.. code-block:: groovy
    
    task count << {
        4.times { print "$it" }
    }

:strong:`gradle -q upper` 的输出

.. code-block:: groovy

    > gradle -q count
    0 1 2 3


任务的依赖
------------------------------------------------------------    

也许你已经猜到了，你可以定义任务之间的依赖关系。

.. code-block:: groovy
    
    task hello<< {
        println 'Hello world!'
    }

    task intro(dependsOn: hello ) << {
        println "I'm Gradle"
    }


:strong:`gradle -q upper` 的输出

.. code-block:: groovy

    > gradle -q intro 
    Hello world!
    I'm Gradle

在给一个任务添加依赖时，所依赖的任务不一定已经存在。

Example 5.7 懒依赖 -- 所依赖的任务还不存在


.. code-block:: groovy
    
    task taskX( dependsOn:'taskY') << {
        println 'taskX'
    }

    task taskY << {
        println 'taskY'
    }


.. code-block:: groovy

    > gradle -q taskX
    taskY
    taskX

依赖于taskY的taskX是在taskY之前申明的。在多项目构建中这个很重要。我们将在 第14.4 给任务加上依赖 一章中详细讨论。

请注意，你不能在任务定义前用shortcut notation来引用他。


动态任务
------------------------------------------------------------    

Groovy的能力远不止定义一个任务。还能做其他，例如你还能动态的创建任务。

动态创建任务

.. code-block:: groovy
    
    4.times { counter ->

        task "task$counter" << {
            println 'I'm task number $counter"
        }

**gradle -q task1** 的输出为

.. code-block:: groovy

    > gradle -q task1
    I'm task number 1

操作已存在的任务
------------------------------------------------------------    

和Ant不同，gralde中的任务一旦被创建，就可以通过一个API来操作他们。 例如你可以为他们添加依赖关系。

通过API来操作任务-- 添加依赖

.. code-block:: groovy
    
    4.times { counter ->

        task "task$counter" << {
            println 'I'm task number $counter"
        }

        task0.dependsOn task2, task3

*gradle -q task1* 的输出为

.. code-block:: groovy

    > gradle -q task1
    I'm task number 2
    I'm task number 3
    I'm task number 0

或者你可以为已存在的任务添加行为。

通过API来操作任务-- 添加行为

.. code-block:: groovy

    task hello << {
        println 'Hello Earth'
    }
    hello.doFirst {
        println 'Hello Venus'
    }
    hello.doLast {
        println 'Hello Mars'
    }
    hello << {
        println 'Hello Jupiter'
    }

:strong:`gradle -q hello` 的输出结果

.. code-block:: groovy

    > gradle -q hello
    Hello Venus
    Hello Earth
    Hello Mars
    Hello Jupiter

每个任务都有一个动作（Action）列表，在执行一个任务的时候会顺序的从该列表获取动作（Action）并执行。doFirst和doLast就是往这个列表的头部和尾部添加一个动作。<< 操作符其实是doLast的简写。 可以多次调用doFirst和doLast。
    

快捷符号
------------------------------------------------------------    

在前面的例子中也许你已经注意到了，你可以很方便的获取已经存在的任务。那是因为每个任务都是构建脚本的一个属性。

以构建脚本属性的方式获取任务。


.. code-block:: groovy

    task hello << {
        println 'Hello world!'
    }
    hello.doLast{
        println 'Greetings from the $hello.name task.'
    }

:strong:`gradle -q hello` 的输出结果

 .. code-block:: groovy

    > gradle -q hello
    Hello world!
    Greetings from the hello task.





任务的动态属性
------------------------------------------------------------    

使用Ant任务
------------------------------------------------------------    

使用方法
------------------------------------------------------------    

默认任务
------------------------------------------------------------    

使用DAG配置
------------------------------------------------------------    

总结
------------------------------------------------------------    

.. [2] 这一行为可以通过命令选项来改变。
