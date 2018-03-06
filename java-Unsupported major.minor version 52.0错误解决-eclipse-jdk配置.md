# java-Unsupported major.minor version 52.0错误解决
eclipse版本设置不对, 低版本不能兼容高版本

eclipse中:
1. windows -> preferences -> java -> Complier -> 设为一致版本
2.  windows -> preferences -> java -> Installed JREs  -> 设为一致版本
3.  项目右键(或core包) -> (properties)build path  -> configure build path -> java build path -> libraries  ->  设为一致版本
4.  项目右键(或core包) -> (properties) -> java compiler ->  设为一致版本
5.  项目右键(或core包) -> (properties) ->project facets ->  设为一致版本

## 相关解释
complier 是全局的解析
installed jres 可以理解为全局常量, 有些设置会直接默认这个常量, 如项目的jdk
build path 单独某个项目的运行环境
java compiler : 单独某个项目的编译环境
project facets: eclipse(其他Ide)的配套组件

## build path 和complier 的区别:
+ build path 是运行环境
+ compiler是提示报错信息和编译时的jdk版本


