# Lombok八股

------

## Lombok的内部原理

**GPT的回答：**

Lombok 是一个用于 Java 编程的库，它通过注解简化了样板代码的编写，比如 getter、setter、构造器、equals、hashCode、toString 等常见方法的自动生成。Lombok 的工作原理是基于注解处理器和编译时字节码操作，它通过修改 AST（抽象语法树）来动态生成或修改代码，而无需手动编写这些方法。

Lombok 的内部原理可以分为几个部分：

**1. 注解处理器（Annotation Processor）**

Lombok 通过实现 JSR 269（Java Annotation Processing API）的注解处理器来插入编译流程。注解处理器允许在 Java 源代码编译期间，扫描和处理特定的注解。Lombok 使用它来在编译过程中生成或修改 Java 类文件。

当使用 Lombok 注解（例如 `@Getter`、`@Setter`）时，Lombok 的注解处理器会拦截这些注解，并通过操纵 AST 自动为你生成所需的代码。

**2. 抽象语法树（AST）修改**

Lombok 使用 Java 的编译工具（如 Eclipse APT 或 javac APT）来修改 AST。AST 是 Java 编译器在编译期间生成的源代码的树形结构表示。

Lombok 在注解处理器执行时，通过修改 AST 在编译阶段插入或修改方法、字段、构造器等。例如：

- 对于 `@Getter`，Lombok 会检查是否有对应的 getter 方法存在，如果没有，它会在 AST 中插入一个生成的 getter 方法。
- 对于 `@Data` 注解，Lombok 会为所有字段生成 getter、setter、equals、hashCode 和 toString 方法。

这种修改不会影响源代码文件，而只在编译时对字节码文件进行变更。

**3. 字节码生成**

Lombok 实际上不会改变源代码本身，它修改的是编译器内部的 AST，并在最终生成的字节码中包含所需的代码。这意味着你可以在代码中不显式编写 getter、setter 等方法，但它们会在编译生成的 `.class` 文件中出现。

**其他文章：**[Lombok原理就是这么简单！-CSDN博客](https://blog.csdn.net/ly279481968/article/details/108000799?ops_request_misc=%7B%22request%5Fid%22%3A%2227F983CE-A266-468B-99E3-9082D0682CCE%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fall.%22%7D&request_id=27F983CE-A266-468B-99E3-9082D0682CCE&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-7-108000799-null-null.142^v100^pc_search_result_base2&utm_term=lombok原理&spm=1018.2226.3001.4187)

[【工具篇】Lombok 介绍及使用（详细教程）-CSDN博客](https://blog.csdn.net/m0_64338546/article/details/131846969?ops_request_misc=&request_id=&biz_id=102&utm_term=lombok原理&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-7-131846969.142^v100^pc_search_result_base2&spm=1018.2226.3001.4187)