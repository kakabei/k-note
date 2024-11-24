# 资料

 [Rust 程序设计语言](https://rust.bootcss.com/title-page.html#rust-%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1%E8%AF%AD%E8%A8%80) 

[Rust 编码规范 V 1.0 beta](https://rust-coding-guidelines.github.io/rust-coding-guidelines-zh/overview.html)

[https://github.com/kakabei/reference/blob/main/docs/rust.md](https://github.com/kakabei/reference/blob/main/docs/rust.md)

[https://github.com/sunface/rust-course?tab=readme-ov-file](https://github.com/sunface/rust-course?tab=readme-ov-file)

# 安装

安装之后的第一个程序就报错了

```sh
rustc hello-work.rs
error: linking with `cc` failed: exit status: 1
......
note: xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools), missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun
```



原因是 `xcode-select ` 没有安装， 重新安装一下 brew 。

```sh
https://mac.install.guide/commandlinetools/3
```

# 安装 vscode 的 rust 插件

社区驱动的 `rust-analyzer 比官方的 rust 好用.

其他几个好用的插件：

1. `Even Better TOML`，支持 .toml 文件完整特性
2. `Error Lens`, 更好的获得错误展示
3. `One Dark Pro`, 非常好看的 VSCode 主题
4. `CodeLLDB`, Debugger 程序

# cargo 

`cargo` 提供了一系列的工具，从项目的建立、构建到测试、运行直至部署，为 Rust 项目的管理提供尽可能完整的手段。

创建：

```sh
$ cargo new cargo-test
$ cd cargo-test
$ tree
.
├── .git
├── .gitignore
├── Cargo.toml
└── src
    └── main.rs
```

 `git` 也同时创建了。

运行项目：cargo run 

```sh
 cargo-test git:(master) ✗ cargo run
   Compiling cargo-test v0.1.0 (/Users/kane/Documents/note/rust-note/code/cargo-test)
    Finished dev [unoptimized + debuginfo] target(s) in 1.31s
     Running `target/debug/cargo-test`
Hello, world!
```

这是 debug 模式下运行，如果想在release下，就要添加参数，如：

```sh 
cargo run --release
cargo build --release
```

**cargo check**
`cargo check` 是我们在代码开发过程中最常用的命令，它的作用很简单：快速的检查一下代码能否编译通过。因此该命令速度会非常快，能节省大量的编译时间。

```sh
cargo check
    Checking cargo-test v0.1.0 (/Users/kane/Documents/note/rust-note/code/cargo-test)
    Finished dev [unoptimized + debuginfo] target(s) in 0.06s
```

**Cargo.toml 和 Cargo.lock**

`Cargo.toml` 和 `Cargo.lock` 是 `cargo` 的核心文件，它的所有活动均基于此二者。

`Cargo.toml` 是 `cargo` 特有的**项目数据描述文件**。它存储了项目的所有元配置信息，如果 Rust 开发者希望 Rust 项目能够按照期望的方式进行构建、测试和运行，那么，必须按照合理的方式构建 `Cargo.toml`。
    
`Cargo.lock` 文件是 `cargo` 工具根据同一项目的 `toml` 文件生成的**项目依赖详细清单**，因此我们一般不用修改它。
`Cargo.toml` 从用户的角度出发来描述项目信息和依赖管理，因此它是由用户来编写
`Cargo.lock`包含了依赖的精确描述信息，它是由 `Cargo` 自行维护，因此不要去手动修改

Cargo.toml 文件内容：

```ini
[package]
name = "cargo-test"
version = "0.1.0"
edition = "2021"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
```

**定义项目依赖**

在 Cargo.toml 是的 `[dependencies]` 定义。

使用 `cargo` 工具的最大优势就在于，能够对该项目的各种依赖项进行方便、统一和灵活的管理。
在 `Cargo.toml` 中，主要通过各种依赖段落来描述该项目的各种依赖项：

- 基于 Rust 官方仓库 `crates.io`，通过版本说明来描述
- 基于项目源代码的 git 仓库地址，通过 URL 来描述
- 基于本地项目的绝对路径或者相对路径，通过类 Unix 模式的路径来描述

格式如下：
```toml 
[dependencies]
rand = "0.3" 
hammer = { version = "0.5.0"} 
color = { git = "https://github.com/bjz/color-rs" } 
geometry = { path = "crates/geometry" }
```



# 常见编程概念

## 变量与可变性



## 数据类型

1、整型
2、浮点型
3、布尔类型
4、字符
5、元组
6、数组

## 函数

` fn` 关键字，它用来声明新函数。

Rust 代码中的函数和变量名使用 *snake case* 规范风格。在 snake case 中，所有字母都是小写并使用下划线分隔单词

# 所有权

所有权的规则:

1. Rust 中的每一个值都有一个被称为其 **所有者**（_owner_）的变量。
2. 值有且只有一个所有者。
3. 当所有者（变量）离开作用域，这个值将被丢弃。

栈与堆

浅拷贝 和 深拷贝。 

# 结构体

结构体的定义和结构体的实例化例子如下： 

```rust
struct User {
    username: String,
    email: String,
    sign_in_count: u64,
    active: bool,
}
let mut user1 = User {
    email: String::from("someone@example.com"),
    username: String::from("someusername123"),
    active: true,
    sign_in_count: 1,
};
user1.email = String::from("anotheremail@example.com");
```

整个实例必须是可变的；Rust 并不允许只将某个字段标记为可变。另外需要注意同其他任何表达式一样，我们可以在函数体的最后一个表达式中构造一个结构体的新实例，来隐式地返回这个实例

```rust
fn build_user(email: String, username: String) -> User {
    User {
        email: email,
        username: username,
        active: true,
        sign_in_count: 1,
    }
}
// 变量与字段同名时的字段初始化简写语法

fn build_user(email: String, username: String) -> User {
    User {
        email,
        username,
        active: true,
        sign_in_count: 1,
    }
}
```

**结构体更新语法**（_struct update syntax_）

```rust

struct User {
    username: String,
    email: String,
    sign_in_count: u64,
    active: bool,
}

let user1 = User {
    email: String::from("someone@example.com"),
    username: String::from("someusername123"),
    active: true,
    sign_in_count: 1,
};

let user2 = User {
    email: String::from("another@example.com"),
    username: String::from("anotherusername567"),
    active: user1.active,
    sign_in_count: user1.sign_in_count,
};

// 结构体更新语法 用法

let user2 = User {
    email: String::from("another@example.com"),
    username: String::from("anotherusername567"),
    ..user1
};

```

使用结构体更新语法，我们可以通过更少的代码来达到相同的效果。`..` 语法指定了剩余未显式设置值的字段应有与给定实例对应字段相同的值。

 **元组结构体**（_tuple structs_）
 
 组结构体有着结构体名称提供的含义，但没有具体的字段名，只有字段的类型。当你想给整个元组取一个名字，并使元组成为与其他元组不同的类型时，元组结构体是很有用的，这时像常规结构体那样为每个字段命名就显得多余和形式化了。

```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

let black = Color(0, 0, 0);
let origin = Point(0, 0, 0);
```

注意 `black` 和 `origin` 值的类型不同，因为它们是不同的元组结构体的实例。

你定义的每一个结构体有其自己的类型，即使结构体中的字段有着相同的类型。
 一 个获取 `Color` 类型参数的函数不能接受 `Point` 作为参数，即便这两个类型都由三个 `i32` 值组成。
 在其他方面，元组结构体实例类似于元组：可以将其解构为单独的部分，也可以使用 `.` 后跟索引来访问单独的值，等等。

**结构体数据的所有权**

**注解**


方法语法

一个 `Rectangle`  结构体上的 `area` 方法：


```rust
#[derive(Debug)]
struct Rectangle { 
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
	
	fn can_hold(&self, other: &Rectangle) -> bool {
        self.width > other.width && self.height > other.height
    }
}

fn main() {
    let rect1 = Rectangle { width: 30, height: 50 };

    println!(
        "The area of the rectangle is {} square pixels.",
        rect1.area()
    );
}

```

**关联函数**

`impl` 块的另一个有用的功能是：允许在 `impl` 块中定义 **不** 以 `self` 作为参数的函数。这被称为 **关联函数**（_associated functions_）

```rust
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
	// 关联函数，不以地self 为入参
    fn square(size: u32) -> Rectangle {
        Rectangle { width: size, height: size }
    }
}
```

# 包和 crate

Rust 有许多功能可以让你管理代码的组织，包括哪些内容可以被公开，哪些内容作为私有部分，以及程序每个作用域中的名字。这些功能。这有时被称为 “模块系统（the module system）”，包括：

- **包**（_Packages_）： Cargo 的一个功能，它允许你构建、测试和分享 crate。
- **Crates** ：一个模块的树形结构，它形成了库或二进制项目。
- **模块**（_Modules_）和 **use**： 允许你控制作用域和路径的私有性。
- **路径**（_path_）：一个命名例如结构体、函数或模块等项的方式