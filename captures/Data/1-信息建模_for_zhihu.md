### 信息建模 Information Modeling

本书讨论如何为一个业务领域建模其 *信息需求（Information Requirements）*，使得建模结果既能被业务用户轻松理解，又能自动生成用于存储该信息的数据库结构。通常情况下, 信息模型会被称为数据模型，但信息和数据是两者不同的术语, 信息为添加了*语义（Semantics）*或 *意义（Meaning）*的数据的数据，而数据本身可能只是一堆数字或者是字符串。如图1.1(a)所示的表格片段中的数据。作为未被解读的语法(syntax)，这可以被赋予无限多种可能的含义。例如，x 和 y 可能代表分别装有10支和100支铅笔的盒子，或者代表长度为10厘米和100厘米的线段，又或者代表为10公斤和100公斤机器人的质量，等等。

图1.1(b)提供的信息稍微多一点。我们大多数人会认为这表达了数字100是10的平方这一事实，尽管原则上它可能意味着其他意思（例如，编号为10的建筑物位于100号广场）。即使说其所含的是数字100是10的平方，如果没有约定所使用的记数法，其语义仍然是模糊的。在默认情况下，100和10常被假定为阿拉伯十进制记数法，但示例数据同样符合二进制记数法（二进制数字“10”和“100”表示由十进制数字“2”和“4”所表示的数字）同样也是语义模糊。当然，如果我们在数据中添加第二行数据对（5, 25），就将排除二进制的解释。

现在考虑图1.1(c)。您会如何用语言表述此示例所传达的事实？大多数人会说：“患者编号（PatientNumber）为10的患者，其体温为100度”。这依赖于对相关业务领域的背景知识（例如，知道患者是通过患者编号来标识的，并且该温度是患者的体温）。

<img src="https://raw.githubusercontent.com/Leamon-Lee/orm-learning/refs/heads/master/captures/Data/1-信息建模/1.png" alt="1" style="zoom: 25%;" />

**图1.1 这些示例旨在传达什么信息？What information is intended to be conveyed by these examples?**

当然，除非我们知道所使用的温标，否则这仍然是模糊的，这很可能指华氏温标，因为100摄氏度和100开尔文对于活着的人来说都太极端了。为了避免测量量可能引起的误解，相关计量单位应始终明确说明，如图1.1(d)所示。因此，预期的事实可以表述为：“患者编号（PatientNumber）为10的患者体温（Temperature）为100华氏度（Fahrenheit Degrees）”。简而言之，需要熟悉业务领域的人为数据添加语义，从而将数据转化为信息。

较好的信息建模关键在于，采用业务用户容易理解的概念来表达预期的语义。满足此要求的信息模型称为*概念模型（Conceptual Model）*[^1]。“*论域（Universe of Discourse，UoD）*”这个术语用于指代业务领域中我们希望讨论的那些方面。因此，概念模型是用自然的、人类易于理解的概念来表达相关论域的模型。当模型完整时，它包含了所有与之相关的结构细节。任何模型（无论是否概念模型）的结构都称为*模式（Schema）*，因此概念模型的结构是概念模式。符合该模式的一组对象实例和事实实例，构成该模式的一个实例集。如图1.2(a)所示，模型（Model）是模式及其数据实例集的组合，其中数据实例集可以是空的[^2]。

在开发信息系统时，信息可以在不同的层面进行处理，如图1.2(b)所示：

<img src="https://raw.githubusercontent.com/Leamon-Lee/orm-learning/refs/heads/master/captures/Data/1-信息建模/2.png" alt="2" style="zoom:25%;" />

**图 1.2 信息系统开发中涉及的模型与层面 Models and levels involved in information systems development**

- ***概念层（Conceptual Level）***：在这里，模型（Models）和查询（Queries）以人类自然理解的概念（例如，实体（Entities）、属性（Attributes）、关系（Relationships））来构建。
- ***逻辑层（Logical Level）***：在这里，信息模型和查询是根据逻辑数据模型（Logical Data Model）（例如，关系模型（Relational）、面向对象模型（Object-Oriented）、层次模型（Hierarchic）、演绎模型（Deductive））所支持的数据与操作的抽象结构来构建。例如，关系模型将事实聚类到表（Tables）中，并使用主键（Primary Key）和外键约束（Foreign Key Declarations）等对它们进行约束。
- ***物理层（Physical Level）***：在这里，选择一个特定的数据库管理系统（Database Management System, DBMS）和/或编程语言来实现逻辑层的结构，并进行性能调优。例如，关系模型可以在 SQL Server、DB2 或 Oracle 中实现为数据库模型（Database Model）。物理层结构也可以被划分为瞬态（内存中）结构（Transient/In-Memory Structures）和持久化（在数据库中）结构（Persistent (Database) Structures），以及它们之间的映射。
- ***外部层（External Level）***：在这里，人类通过基于表单的（Forms-Based）或图形用户界面(GUI)直接与系统交互。不同的用户组可能被分配不同的对底层数据的访问权限，用户可以通过界面输入查询，查询结果则显示在屏幕上或出现在打印报告中。

在实践中，并非所有信息系统都使用所有四个层面进行开发，并且某些结构可能跨越多个层面（例如，UML 中的类模型（Class Models in UML）可以说是结合了概念层、逻辑层和物理层的方面）。此外，技术用户（Technical Users）可能会直接在物理层与系统交互。然而，理想情况下，建模应首先在概念层完成，与业务领域专家在那里进行验证，然后通过将概念结构自动或至少半自动地转换到其他层面来进行*前向工程（Forward Engineering）*。这个整体过程称为模型驱动工程）。

*对象角色建模（Object-Role Modeling, ORM）*（本书的重点）是一种模型驱动工程方法，它从用户熟悉的任何外部形式呈现的所需信息和查询的典型示例开始，然后在概念层用简单事实对这些示例进行*表述（Verbalized）*，这些简单事实用*受控自然语言（Controlled Natural Language）*表达——一种受限制且明确无歧义的自然语言变体，其语义容易被人类理解，同时也是形式化的，因此它可以用来将结构自动映射到较低层面进行实现，如图1.3所示。作为这一整体过程的一部分，本书讨论了如何创建 ORM 模型以及如何将它们映射到关系数据库模型。ORM 模型也可以映射到其他类型的模型（对象导向型、演绎型等）。

<img src="https://raw.githubusercontent.com/Leamon-Lee/orm-learning/refs/heads/master/captures/Data/1-信息建模/3.png" alt="3" style="zoom:25%;" />

**图1.3 基于示例表述的模型驱动工程 (Model-Driven Engineering Based on Verbalization of Examples)**

[^1]: 有些作者使用“概念模型”一词时，含义较为宽泛，仅涵盖业务领域的主要方面
[^2]: 有些作者将“模型(model)”一词视为“模式(schema)”的同义词，但我们认为模型(model)也可以包含数据集（人口数据）。
