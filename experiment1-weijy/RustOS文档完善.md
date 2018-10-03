# MacOS上RustOS工作复现
我在macOS上复现了王润基同学的RustOS工作，整理并完成了在macOS上配置相关环境的文档如下：

## Rust环境搭建
Rust编译的安装和下载相对比较容易，执行官网上提供的命令：

curl https://sh.rustup.rs -sSf | sh

将得到一个叫做rustup的工具，该工具能够安装Rust相关的一整套工具链，包括编译器，标准库，cargo

rustup self update //更新rustup本身
rustup update //更新工具链
rustup self uninstall //卸载rust所有程序

Rust语言使用了多渠道发布的策略，在本工程中使用的是nightly版本，这个版本是每天在主版本上自动创建出来的版本，这个版本上的功能最多，更新最快。
可以使用下面的方法来安装nightly

rustup install nightly //安装nightly版本的编译工具链
rustup default nightly //设置默认工具链是nightly版本

可以使用下面的方法来更新nightly

rustup update nightly //更新rustup编译器到nightly的最新版本

## 安装Rust依赖项cargo-xbuild, bootimage
cargo-xbuild、bootimage是两个Cargo工具，主要用于Rust内核开发，直接使用二者官网上的安装方法在macOS上安装即可（使用cargo install安装）

## 安装 QEMU 模拟器
在macOS上建议使用brew install qemu来安装qemu模拟器

## 复现Riscv32框架的RustOS
在macOS上复现Riscv32框架时遇见的最大的问题就是配置RiscV64 GNU工具链，我曾经尝试自己从RiscV官网下载RiscV工具链自己编译，最终因为macOS的gcc版本管理过于混乱而失败。。因此我采用的还是原来文档中的 https://www.sifive.com/products/tools/ 下载了编译好的RiscV64工具链到本地文件夹 $LOCAL_DIR ，然后配置环境变量：

vim ~/.bash_profile

在文件中添加

export PATH="/Users/victor/$LOCAL_DIR/bin:$PATH"（注意：在macOS中配置环境变量时不得在“=”的两边加空格，否则会出现不识别的问题）

source ~/.bash_profile //使得刚刚配置的环境变量生效

配置完成RiscV工具链之后就可以在mac中正常使用RustOS了

## 复现x86_64框架的RustOS
在macOS中，复现x86_64框架的RustOS可能遇见一个很致命的问题" 'ld.bfd' not found "，这是因为cargo安装的bootimage在编译bootloader时使用了linux-gnu工具链的中的ld.bfd，而有些macOS系统和linux-gnu工具链不兼容，需要自己下载并配置该工具链。
下载linux-gnu工具链时，强烈不建议使用macport，因为macport安装的链接器有可能会和原来系统已有的编译器产生冲突，带来严重的后果（原本已经可以正常编译的部分也出错），建议使用 https://github.com/rust-osdev/bootloader/issues/8 中推荐的方法来安装linux-gnu：

brew tap SergioBenitez/osxct //配置brew tap
brew install x86_64-unknown-linux-gnu //使用brew install安装linux-gnu

安装好linux-gnu之后，需要在原来编译bootloader的选项中将ld.bfd修改为新安装的链接器文件名，首先进入cargo安装了bootloader的文件夹$DIR中：

vim $DIR/x86_64-bootloader.json //修改x64_64bootloader的编译配置

在该文件中有一个：

"linker" : "ld.bfd"

将其修改为：

"linker" : "x86_64-unknown-linux-gnu-ld.bfd"

最后再次编译x86_64框架的RustOS即可正确运行