# go随手笔记

## 2020/11/15

* 遇到了 `gob error encoding body has no exported fields` 的报错，查了一下，是结构体的字段首字母没有大写，默认首字母没大写是private字段，也就是在其他的包没有办法找到这个字段，命名的时候要养成首字母大写的习惯。
