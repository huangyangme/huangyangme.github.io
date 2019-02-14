#网页字体设置

##无衬线体（Mac、PC）

`font-family: "Helvetica Neue", Helvetica, Arial, Hiragino Sans GB, Microsoft YaHei, sans-serif;`

##衬线体（Mac、PC）

`font-family: Georgia, "Times New Roman", Times, "Songti SC", "SimSun", sans-serif;`

##无衬线体（Pad、Phone）

`font-family: "Helvetica Neue", Helvetica, STHeiTi, sans-serif;`

iOS 4.0+ 使用英文字体 Helvetica Neue，之前的iOS版本降级使用 Helvetica。中文字体设置为华文黑体 STHeiTi。 需补充说明，华文黑体并不存在 iOS 的字体库中(http://support.apple.com/kb/HT5484?viewlocale=en_US)， 但系统会自动将「华文黑体STHeiTi」兼容命中系统默认中文字体「黑体-简」或「黑体-繁」

>Heiti SC Light 黑体-简 细体 Heiti SC Medium 黑体-简 中黑 Heiti TC Light 黑体-繁 细体 Heiti TC Medium 黑体-繁 中黑
原生 Android 下中文字体与英文字体都选择默认的无衬线字体

>4.0 之前版本英文字体原生 Android 使用的是 Droid Sans，中文字体原生 Android 会命中 Droid Sans Fallback 4.0 之后中英文字体都会使用原生 Android 新的 Roboto 字体 其他第三方 Android 系统也一致选择默认的无衬线字体