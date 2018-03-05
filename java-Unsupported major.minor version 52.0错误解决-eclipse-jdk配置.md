# java-Unsupported major.minor version 52.0错误解决
eclipse版本设置不对, 低版本不能兼容高版本

eclipse中:
1. windows -> preferences -> java -> Complier -> 设为一致版本
2.  windows -> preferences -> java -> Installed JREs  -> 设为一致版本
3.  项目右键(或core包) -> (properties)build path  -> configure build path -> java build path -> libraries  ->  设为一致版本
4.  项目右键(或core包) -> (properties) -> java compiler ->  设为一致版本
5.  项目右键(或core包) -> (properties) ->project facets ->  设为一致版本