# SpringBoot全局异常返回错误页面的处理

## 步骤

- 配置@ControllerAdvice和@ExceptionHandler
- 若Handler函数返回ModelAndView,则抛出捕获异常的controller方法的返回类型也
需要为ModelAndView才能实现捕获异常后跳转