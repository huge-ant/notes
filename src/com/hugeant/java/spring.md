> servlet生命周期
1. 实例化
2. 初始化init方法
3. 接收请求service
4. 销毁distroy

> springBean 生命周期
1. 实例化bean
2. 设置对象属性（自动注入）
3. 处理aware接口
4. beanPostProcessorAfter
5. 执行init-mothod方法
6. beanPostProcessorBefore
7 DisposableBean销毁（destroy-method）