---
title: Spring源码阅读之spring-core
tags:
---

springIOC的两种实现方式

依赖查找
  1.根据bean名称查找
    实时查找
    延迟查找
      ObjectFactory
  2.根据bean类型查找
    单个bean查找
    bean集合查找
  3.根据注解查找

依赖查找的前世今生

依赖注入
  1.根据bean名称注入
  2.根据bean类型注入
  3.注入容器内建bean对象
  4.注入非bean对象
  5.注入类型
依赖来源
  1.自定义bean
  2.容器内建bean
  3.容器内建依赖
IOC配置元信息
  1.基于xml文件
  2.基于properties文件
  3.基于Java注解
  4.基于JavaAPI
IOC容器配置
  1.基于xml
  2.基于Java注解
  3.基于JavaAPI
外部化配置

BeanFactory和ApplicationContext到底哪个才是IOC容器？
The org.springframework.beans and org.springframework.context packages are the basis for Spring Framework’s IoC container. The BeanFactory interface provides an advanced configuration mechanism capable of managing any type of object. ApplicationContext is a sub-interface of BeanFactory. It adds:

- Easier integration with Spring’s AOP features
- Message resource handling (for use in internationalization)
- Event publication
- Application-layer specific contexts such as the WebApplicationContext for use in web applications.

You should use an ApplicationContext unless you have a good reason for not doing so, with GenericApplicationContext and its subclass AnnotationConfigApplicationContext as the common implementations for custom bootstrapping. These are the primary entry points to Spring’s core container for all common purposes: loading of configuration files, triggering a classpath scan, programmatically registering bean definitions and annotated classes, and (as of 5.0) registering functional bean definitions.

Because an ApplicationContext includes all the functionality of a BeanFactory, it is generally recommended over a plain BeanFactory, except for scenarios where full control over bean processing is needed. Within an ApplicationContext (such as the GenericApplicationContext implementation), several kinds of beans are detected by convention (that is, by bean name or by bean type — in particular, post-processors), while a plain DefaultListableBeanFactory is agnostic about any special beans.

For many extended container features, such as annotation processing and AOP proxying, the BeanPostProcessor extension point is essential. If you use only a plain DefaultListableBeanFactory, such post-processors do not get detected and activated by default. This situation could be confusing, because nothing is actually wrong with your bean configuration. Rather, in such a scenario, the container needs to be fully bootstrapped through additional setup.

In short, the BeanFactory provides the configuration framework and basic functionality, and the ApplicationContext adds more enterprise-specific functionality.

ApplicationContext is a BeanFacotry
ApplicationContext has a BeanFactory

ApplicationContext除了IOC容器的角色，还提供
- 面向切面
- 配置元信息
- 资源管理
- 事件
- 国际化
- 注解
- Environment抽象


refresh方法，ioc容器启动


1. Spring Bean：spring框架中定义bean的配置元信息接口
   1. Bean的类名
   2. Bean行为配置元素（作用域、自动绑定的模式、生命周期回调）
   3. 其它Bean的引用，也就是依赖
   4. 配置设置
2. BeanDefinition元信息
3. 命名Spring Bean
4. Spring Bean 别名
5. 注册
6. 实例化
7. 初始化
8. 延迟初始化
9.  销毁
10. 垃圾回收

BeanDefinition
1. get
2. set
3. has
4. is
5. fields
   1. ROLE_APPLICATION: 
   2. ROLE_INFRASTRUCTURE：
   3. ROLE_SUPPORT：
   4. SCOPE_PROTOTYPE：作用域标识符，原型
   5. SCOPE_SINGLETON：作用域标识符，单例

BeanDefinition构建
1. 通过BeanDefinitionBuilder
2. 通过AbstractBeanDefinition以及派生类

命名Spring Bean
1. 指定名称(xml)
2. 不指定名称(注解)

Spring Bean别名

注册Spring Bean
1. XML配置元信息
2. Java注解配置元信息
   1. @Bean
   2. @Component
   3. @Import
3. JavaAPI配置元信息
   1. 命名方式BeanDefinitionRegistry#registerBeanDefinition
   2. 非命名方式BeanDefinitionReaderUtils#registerWithGeneratedName
   3. 配置类的方式AnnotatedBeanDefinitionReader.register

实例化Spring Bean
1. 常规方式
2. 特殊方式

初始化Spring Bean
1. @PostConstruct
2. 实现InitializingBean接口的afterPropertiesSet()方法
3. 自定义初始化方法
   1. init-method
   2. AbstractBeanDefinition#setInitMethodName

延迟初始化（延迟加载）

  构造器注入
  setter注入 

依赖查找的前世今生

###############
单一类型查找
1. JNDI--javax.naming.Context#lookup
2. JavaBeans--java.beans.beancontext.BeanContext

集合类型查找
java.beans.beancontext.BeanContext

层次性依赖查找

################











###########################################################

Environment:
Spring中的Environment接口从两个方面来描述应用运行的环境：profiles（配置文件）、properties（属性）。

Environment继承了PropertyResolver接口，PropertyResolver接口定义了一些与属性相关的操作，Environment具有为用户配置property source（属性源），并使用这些属性源的作用。

Spring中的Profile可以理解为我们在Spring容器中所定义的Bean的逻辑组名称，只有当这些Profile被激活的时候，才会将Profile中所对应的Bean注册到Spring容器中。[参考](https://www.cnblogs.com/SummerinShire/p/6392242.html)


```java
  //获取激活的配置文件集合
  String[] getActiveProfiles();
  //获取默认的配置文件集合，当environment没有配置激活的配置文件时，就会采用默认的配置文件
	String[] getDefaultProfiles();
  //返回一个或多个给定的配置文件是否处于活动状态，或者在没有明确的活动配置文件的情况下，一个或多个给定的配置文件是否包含在一组默认配置文件中。
	boolean acceptsProfiles(String... profiles);
  //判断给定的profiles是否与active profiles匹配
	boolean acceptsProfiles(Profiles profiles);
```

ConfigurableEnvironment:
继承Environment和ConfigurablePropertyResolver接口，Configurable修饰说明这个接口定义了一些可以对Environment进行配置的功能

```java
  void setActiveProfiles(String... profiles);

	void addActiveProfile(String profile);

	void setDefaultProfiles(String... profiles);
  //获取Environment中所有的数据源
	MutablePropertySources getPropertySources();
  //获取系统变量，TODO看完具体实现再来理解这句话Therefore, it is recommended that this method not be used directly unless bypassing other property sources is expressly intended.
	Map<String, Object> getSystemProperties();
  //TODO得看具体实现
	Map<String, Object> getSystemEnvironment();
  //将给定的父环境的活动配置文件、默认配置文件和属性源附加到此（子）环境各自的集合中。这个方法的作用不是很了解
	void merge(ConfigurableEnvironment parent);
```


PropertyResolver：
属性解析器，Resolver不只局限于get属性，在get的过程中做了一些其它的操作
1. 通过key获取对应的value，获取不到value时有三种选择：返回null、使用指定的默认值或者抛出一个异常（required的方法说明是必须得找到value，找不到就抛异常），获取到的value可以是String类型的，也可以是指定的一个类型，通过ConversionService进行类型转换
2. 当属性值中有占位符时，可以做替换操作
```java
public interface PropertyResolver {

	boolean containsProperty(String key);

	String getProperty(String key);

	String getProperty(String key, String defaultValue);

	<T> T getProperty(String key, Class<T> targetType);

	<T> T getProperty(String key, Class<T> targetType, T defaultValue);

	String getRequiredProperty(String key) throws IllegalStateException;

	<T> T getRequiredProperty(String key, Class<T> targetType) throws IllegalStateException;

	String resolvePlaceholders(String text);

	String resolveRequiredPlaceholders(String text) throws IllegalArgumentException;

}
```

ConfigurablePropertyResolver：可配置的属性解析器
1. 设置conversionService
2. 设置占位符前缀和后缀
3. 设置占位符和默认值之间的分隔符
4. 设置一些属性是必须存在的
5. 验证设置为必须的属性是否存在，并且对应的属性值不为null，否则抛异常

```java
public interface ConfigurablePropertyResolver extends PropertyResolver {

	ConfigurableConversionService getConversionService();

	void setConversionService(ConfigurableConversionService conversionService);

	void setPlaceholderPrefix(String placeholderPrefix);

	void setPlaceholderSuffix(String placeholderSuffix);

	void setValueSeparator(@Nullable String valueSeparator);

	void setIgnoreUnresolvableNestedPlaceholders(boolean ignoreUnresolvableNestedPlaceholders);

	void setRequiredProperties(String... requiredProperties);

	void validateRequiredProperties() throws MissingRequiredPropertiesException;

}
```

AbstractPropertyResolver:PropertyResolver与ConfigurablePropertyResolver的抽象实现类，大部分方法都实现了，最重要的getProperty方法留到子类实现

PropertySourcesPropertyResolver：上诉接口和抽象类的实现，有一个PropertySources属性，表示属性源，以此作为来源实现了getProperty这个最主要的方法

```java
protected <T> T getProperty(String key, Class<T> targetValueType, boolean resolveNestedPlaceholders) {
		if (this.propertySources != null) {
			for (PropertySource<?> propertySource : this.propertySources) {
				if (logger.isTraceEnabled()) {
					logger.trace("Searching for key '" + key + "' in PropertySource '" +
							propertySource.getName() + "'");
				}
				Object value = propertySource.getProperty(key);
				if (value != null) {
					if (resolveNestedPlaceholders && value instanceof String) {
						value = resolveNestedPlaceholders((String) value);
					}
					logKeyFound(key, propertySource, value);
					return convertValueIfNecessary(value, targetValueType);
				}
			}
		}
		if (logger.isTraceEnabled()) {
			logger.trace("Could not find key '" + key + "' in any property source");
		}
		return null;
	}
```

PropertySources:属性源

public interface PropertySources extends Iterable<PropertySource<?>> {

	default Stream<PropertySource<?>> stream() {
		return StreamSupport.stream(spliterator(), false);
	}

	boolean contains(String name);

	PropertySource<?> get(String name);

}

MutablePropertySources:可变的属性源，维护了一个PropertySource类型的List
```java
private final List<PropertySource<?>> propertySourceList = new CopyOnWriteArrayList<>();
```

PropertySource:
在生产实践中，配置覆盖是解决不同环境不同配置的常用方法。比如用生产服务器上的配置文件覆盖包内的文件，或者使用中心化的配置服务来覆盖默认的业务配置。
相比于profile机制（比如maven的profile、spring boot的profile-specific properties），即不同环境使用不同的配置文件，覆盖的方式更有优势。程序员在开发时不需要关心生产环境数据库的地址、账号等信息，一次构建即可在不同环境中运行，而profile机制需要将生产环境的配置写到项目资源文件中，而且要为不同环境使用不同的构建参数或者运行参数。

ConversionService:主要是单例模型和用一个service来管理一堆转换器的思想

ConverterRegistry
FormatterRegistry：是ConverterRegistry的一种


ConfigurableConversionService

GenericConversionService

DefaultConversionService

ApplicationConversionService

Environment

1. PropertyResolver
2. ConversionService
3. ConverterRegistry
4. PropertySources
5. PropertySource
6. Profiles












BeanPostProcessor

Typically, post-processors that populate beans via marker interfaces or the like will implement postProcessBeforeInitialization, while post-processors that wrap beans with proxies will normally implement postProcessAfterInitialization.