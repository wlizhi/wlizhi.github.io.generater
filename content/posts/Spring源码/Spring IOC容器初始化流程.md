---
title: "Spring IOC容器初始化流程"
date: 2019-11-16T15:15:09+08:00
draft: false
toc: true
tags: ["spring", "源码", "spring源码"]
categories: ["spring源码"]
---

### 1.常见的实现类：
- 容器：<font color="red">AbstractApplicationContext</font> 抽象父类，核心(模板)方法 refresh()。
	- ClassPathXmlApplicationContext XML方式启动。
	- AnnotationConfigWebApplicationContext 注解方式启动，对局部代码进行测试时比较好用。
	- AnnotationConfigServletWebServerApplicationContext SpringBoot启动默认使用的上下文类。

### 2. spring ioc容器初始化的核心方法refresh，AbstractApplicationContext.refresh() 。
1. **外层方法骨架**
```java
	@Override
	public void refresh() throws BeansException, IllegalStateException {
		synchronized (this.startupShutdownMonitor) {
			// Prepare this context for refreshing.
			// 不太重要，预刷新，做一些准备工作。记录了启动时间戳，标记为活动，非关闭状态。
			prepareRefresh();

			/**
			 * TODO 重点：解析xml配置文件，创建beanFactory，包装BeanDefinition
			 */
			// Tell the subclass to refresh the internal bean factory.
			ConfigurableListableBeanFactory beanFactory = obtainFreshBeanFactory();

			// Prepare the bean factory for use in this context.
			// 注册一些对事件、监听器等的支持
			prepareBeanFactory(beanFactory);

			try {
				// 钩子方法，BeanFactory创建后，对BeanFactory的自定义操作。
				// Allows post-processing of the bean factory in context subclasses.
				postProcessBeanFactory(beanFactory);

				// TODO 重点：这里调用了postProcessBeanDefinitionRegistry(registry);springboot中很多激活自动配置的注解都是通过这里导入的。
				// Invoke factory processors registered as beans in the context.
				invokeBeanFactoryPostProcessors(beanFactory);

				// TODO 重点：从beanFactory中获取所有的BeanPostProcessor，优先进行getBean操作，实例化
				// Register bean processors that intercept bean creation.
				registerBeanPostProcessors(beanFactory);

				// 国际化支持
				// Initialize message source for this context.
				initMessageSource();

				// 初始化ApplicationEventMulticaster。 如果上下文中未定义，则使用SimpleApplicationEventMulticaster。
				// Initialize event multicaster for this context.
				initApplicationEventMulticaster();

				// 钩子方法，springBoot中的嵌入式tomcat就是通过此方法实现的
				// Initialize other special beans in specific context subclasses.
				onRefresh();

				// 监听器注册
				// Check for listener beans and register them.
				registerListeners();

				// TODO 重点方法：完成容器中bean的实例化，及代理的生成等操作。
				// Instantiate all remaining (non-lazy-init) singletons.
				finishBeanFactoryInitialization(beanFactory);

				// 完成此上下文的刷新，调用LifecycleProcessor的onRefresh（）方法并发布
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
			}
		}
	}
```