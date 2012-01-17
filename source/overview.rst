.. highlight:: rst

.. _overview:

概述
==============================================================================
功能特点
------------------------------------------------------------------------------
下面列出 Gradle_ 拥有的一些功能。

 :Declarative builds and build-by-convention:
    基于Groovy的高扩展的领域语言（DSL）是Gradle的核心。

 :Language for dependency based programming:
    声明式语言
 :Structure you build:
    结构化你的脚本
 :Deep API:
    深入API
 :Gradle Scales:
    Gradle 的扩展性非常好。
 :多项目构建:
    Gradle 支持多个项目构建。
 :多样的依赖管理:
    不同的团队可能会喜欢不同的依赖管理。
 :Gradle是第一个集成构建工具:
    Ant任务是一等公民。
 :迁移成本低:
    Gradle 可以适配任何结构。
 :Groovy:
    Gradle的构建脚本是用Groovy写的而不是xml。
 :Gradle 包装器:
    Gradle 包装器允许你扩展。
 :免费并且开源:
    Gradle是一个开源项目。他使用 ASL_ 开源协议。

为什么选Groovy
------------------------------------------------------------
我们认为基于动态语言的 DSL_ 要比Xml更适合做构建脚本。为什么在一堆动态语言中偏偏选中Groovy呢? 这是由Gradle活动的上下文所决定的（in the context Gradle is operating in）。 虽然Gradle是通用的构建工具，但是他重点还是Java项目。很显然这些项目的开发人员都了解Java。我们认为构建应该对所有开发人员尽可能的透明。
也许你会说为什么不直接用Java来做构建脚本。这是一个好问题。那样的话构建对所有开发人员将有最高的透明程度，并且拥有最平滑的学习曲线。


.. _DSL: http://en.wikipedia.org/wiki/Domain-specific_language 
.. _ASL: http://www.apache.org/licenses/LICENSE-2.0.html
