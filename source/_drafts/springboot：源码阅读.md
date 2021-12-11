---
title: springboot：源码阅读之refresh方法
tags:
---

StartupStep contextRefresh = this.applicationStartup.start("spring.context.refresh");
ApplicationStartup



// Prepare this context for refreshing.
prepareRefresh();

// Tell the subclass to refresh the internal bean factory.
ConfigurableListableBeanFactory beanFactory = obtainFreshBeanFactory();

// Prepare the bean factory for use in this context.
prepareBeanFactory(beanFactory);

try {
  // Allows post-processing of the bean factory in context subclasses.
  postProcessBeanFactory(beanFactory);

  StartupStep beanPostProcess = this.applicationStartup.start("spring.context.beans.post-process");
  // Invoke factory processors registered as beans in the context.
  invokeBeanFactoryPostProcessors(beanFactory);

  // Register bean processors that intercept bean creation.
  registerBeanPostProcessors(beanFactory);
  beanPostProcess.end();

  // Initialize message source for this context.
  initMessageSource();

  // Initialize event multicaster for this context.
  initApplicationEventMulticaster();

  // Initialize other special beans in specific context subclasses.
  onRefresh();

  // Check for listener beans and register them.
  registerListeners();

  // Instantiate all remaining (non-lazy-init) singletons.
  finishBeanFactoryInitialization(beanFactory);

  // Last step: publish corresponding event.
  finishRefresh();
}

catch (BeansException ex) {
  if (logger.isWarnEnabled()) {
    logger.warn("Exception encountered during context initialization - " +
        "cancelling refresh attempt: " + ex);
  }

  // Destroy already created singletons to avoid dangling resources.
  destroyBeans();

  // Reset 'active' flag.
  cancelRefresh(ex);

  // Propagate exception to caller.
  throw ex;
}

finally {
  // Reset common introspection caches in Spring's core, since we
  // might not ever need metadata for singleton beans anymore...
  resetCommonCaches();
  contextRefresh.end();
}


--------------------------------------------

所以BeanNameAware接口是为了让自身Bean能够感知到，获取到自身在Spring容器中的id属性。

同理，其他的Aware接口也是为了能够感知到自身的一些属性。
比如实现了ApplicationContextAware接口的类，能够获取到ApplicationContext，实现了BeanFactoryAware接口的类，能够获取到BeanFactory对象。