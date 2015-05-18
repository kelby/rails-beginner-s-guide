## ~~Session~~

在文件目录上很有意思，它不是放在 http/ 而是放在单独的 request/ 目录下。

这是因为和其它 http/ 下的模块相比，它不仅被 request 调用，同时它在 middleware Flash 里也被调用。
