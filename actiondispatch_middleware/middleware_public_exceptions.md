**Public Exceptions**

负责'渲染'错误页面这个工作。默认从 `public/` 目录中寻找对应的异常文件，如 500 则渲染 `/public/500.html`

它只处理渲染相关逻辑，只是"定义"，至于要不要使用它做为默认异常处理，由 ShowExceptions 和 ExceptionWrapper 等共同决定。
