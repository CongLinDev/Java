# Spring

## Spring MVC

1. 用户发送请求
2. DispatcherServlet 查找 HandlerMapping 对应的 Handler
3. DispatcherServlet 通过 HandlerAdapter 调用 Handler 返回 ModelAndView
4. DispatcherServlet 使用 ViewResolver 处理 ModelAndView
5. DispatcherServlet 返回结果

## AOP场景

1. 日志
2. 权限控制
3. 链路追踪
4. 缓存
5. 延迟加载
6. 事务管理

## Refresh

1. prepareRefresh() 准备工作，记录下容器的启动时间、标记“已启动”状态、处理配置文件中的占位符
2. obtainFreshBeanFactory() 解析配置文件注册到 BeanFactory（内部是一个 BeanName-> BeanDefinition 的 map）
3. prepareBeanFactory() 设置 BeanFactory 的类加载器，添加几个 BeanPostProcessor，手动注册几个特殊的 bean
4. postProcessBeanFactory() 给子类的扩展点 具体的子类可以添加一些特殊的 BeanFactoryPostProcessor
5. invokeBeanFactoryPostProcessors() 调用 BeanFactoryPostProcessor 的 postProcessBeanFactory(factory)
6. registerBeanPostProcessors() 注册 BeanPostProcessor
7. initMessageSource() 用于国际化
8. initApplicationEventMulticaster() 初始化事件广播器
9. onRefresh() 子类的扩展点具体的子类可以在这里初始化一些特殊的 Bean
10. registerListeners() 注册事件监听器，监听器需要实现 ApplicationListener 接口
11. finishBeanFactoryInitialization() 初始化所有的 singleton beans（lazy-init 的除外）
12. finishRefresh() 广播事件，初始化完成

## Bean 生命周期

1. 扫描得到 BeanDefinition
2. 实例化 BeanFactoryPostProcessor
3. 调用 BeanFactoryPostProcessor 的 postProcessBeanFactory
4. 实例化 BeanPostProcessor
5. 调用 InstantiationAwareBeanPostProcessor 的 postProcessBeforeInstantion() 该方法在目标对象实例化之前调用，如果返回不为null，则会替代目标对象，后续直接跳到16步
6. 推断并执行Bean的构造器
7. 调用 InstantiationAwareBeanPostProcessor 的 postProcessAfterInstantion() 如果返回false，后续直接跳到12步
8. 获取属性 autowireByName / autowireByType
9. 调用 InstantiationAwareBeanPostProcessor 的 postProcessProoertes() 处理注解
10. 调用 InstantiationAwareBeanPostProcessor 的 postProcessProoertyValues() 处理 （弃用）
11. 注入Bean的属性
12. 调用 Aware接口方法 如 BeanNameAware BeanClassLoaderAware BeanFactoryAware
13. 调用 BeanPostProcessor 的 postProcessBeforeInitialization()
14. 调用 InitializingBean 的 afterPropertiesSet()
15. 调用 Bean 的 init-method
16. 调用 BeanPostProcessor 的 postProcessAfterInitialization()
17. 调用 DiposibleBean 的 destroy()
18. 调用 Bean 的 destroy-method

## Bean构造方法推断

### 没有被 @Autowired 修饰的构造器

若只有一个构造函数，直接选用。

若多个构造函数且包含无参构造函数，那么选用无参构造函数。

若多个构造函数且不包含无参构造函数，那么抛异常。

### 有被 @Autowired 修饰的构造器

若只有一个构造函数被 @Autowired 修饰，直接选用。

若多个构造函数被 @Autowired 修饰，且其中只要有一个 @Autowired 的 required = true，那么抛异常。

若多个构造函数被 @Autowired 修饰， 且 required = false 均成立，那么对这些构造函数进行排序。排序规则使用差异度算法，根据继承层级计算。


