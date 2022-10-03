# 1. SpringBoot原理分析

## 1.1 @EnableAutoConfiguration

### 1.1.1 @Import(AutoConfigurationImportSelector.class)

使用@Import导入的类会被Spring加载到IOC容器中

@Import提供4种用法：

1. 导入Bean

   ~~~java
   @Import(User.class)
   ~~~

2. 导入配置类

   ~~~java
   @Import(UserConfig.class)
   
   ~~~

   ~~~java
   @Configuration
   public class UserConfig {
   
       @Bean("user")
       public User user() {
           return new User();
       }
   }
   
   ~~~

3. 导入 ImportSelector 实现类。一般用于加载配置文件中的类

   ~~~java
   @Import(MyImportSelector.class)
   ~~~

   ~~~java
   public class MyImportSelector implements ImportSelector {
       @Override
       public String[] selectImports(AnnotationMetadata annotationMetadata) {
           return new String[]{"com.mszlu.domain.User"};
       }
   }
   ~~~

   

4. 导入 ImportBeanDefinitionRegistrar 实现类。

   ~~~java
   @Import(MyImportBeanDefinitionRegistrar.class)
   ~~~

   ~~~java
    public class MyImportBeanDefinitionRegistrar implements ImportBeanDefinitionRegistrar {
   
           @Override
           public void registerBeanDefinitions(AnnotationMetadata importingClassMetadata, BeanDefinitionRegistry registry) {
               AbstractBeanDefinition beanDefinition = BeanDefinitionBuilder.rootBeanDefinition(User.class).getBeanDefinition();
               registry.registerBeanDefinition("user",beanDefinition);
           }
       }
   ~~~

### 1.1.2 AutoConfigurationImportSelector代码分析

~~~java
@Override
	public String[] selectImports(AnnotationMetadata annotationMetadata) {
		if (!isEnabled(annotationMetadata)) {
			return NO_IMPORTS;
		}
        //获取自动配置相关的信息
		AutoConfigurationEntry autoConfigurationEntry = getAutoConfigurationEntry(annotationMetadata);
		return StringUtils.toStringArray(autoConfigurationEntry.getConfigurations());
	}
~~~

~~~java
protected AutoConfigurationEntry getAutoConfigurationEntry(AnnotationMetadata annotationMetadata) {
		if (!isEnabled(annotationMetadata)) {
			return EMPTY_ENTRY;
		}
		AnnotationAttributes attributes = getAttributes(annotationMetadata);
    	//读取指定的配置文件，获取配置列表 
		List<String> configurations = getCandidateConfigurations(annotationMetadata, attributes);
		configurations = removeDuplicates(configurations);
		Set<String> exclusions = getExclusions(annotationMetadata, attributes);
		checkExcludedClasses(configurations, exclusions);
		configurations.removeAll(exclusions);
		configurations = getConfigurationClassFilter().filter(configurations);
		fireAutoConfigurationImportEvents(configurations, exclusions);
		return new AutoConfigurationEntry(configurations, exclusions);
	}
~~~

~~~java
	protected List<String> getCandidateConfigurations(AnnotationMetadata metadata, AnnotationAttributes attributes) {
        //读取META-INF/spring.factories 中的配置，根据其中的配置找到对应需要加载的配置类
		List<String> configurations = SpringFactoriesLoader.loadFactoryNames(getSpringFactoriesLoaderFactoryClass(),
				getBeanClassLoader());
		Assert.notEmpty(configurations, "No auto configuration classes found in META-INF/spring.factories. If you "
				+ "are using a custom packaging, make sure that file is correct.");
		return configurations;
	}
~~~

~~~properties
//这是spring-boot-autoconfigure包下的META-INF/spring.factories 其中关于Redis的配置
org.springframework.boot.autoconfigure.EnableAutoConfiguration=org.springframework.boot.autoconfigure.data.redis.RedisAutoConfiguration
~~~

~~~java
/*
 * Copyright 2012-2020 the original author or authors.
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *      https://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

package org.springframework.boot.autoconfigure.data.redis;

import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.boot.autoconfigure.condition.ConditionalOnClass;
import org.springframework.boot.autoconfigure.condition.ConditionalOnMissingBean;
import org.springframework.boot.autoconfigure.condition.ConditionalOnSingleCandidate;
import org.springframework.boot.context.properties.EnableConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisOperations;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.core.StringRedisTemplate;

/**
 * {@link EnableAutoConfiguration Auto-configuration} for Spring Data's Redis support.
 *
 * @author Dave Syer
 * @author Andy Wilkinson
 * @author Christian Dupuis
 * @author Christoph Strobl
 * @author Phillip Webb
 * @author Eddú Meléndez
 * @author Stephane Nicoll
 * @author Marco Aust
 * @author Mark Paluch
 * @since 1.0.0
 */
@Configuration(proxyBeanMethods = false)
@ConditionalOnClass(RedisOperations.class)
@EnableConfigurationProperties(RedisProperties.class)
@Import({ LettuceConnectionConfiguration.class, JedisConnectionConfiguration.class })
public class RedisAutoConfiguration {

	@Bean
	@ConditionalOnMissingBean(name = "redisTemplate")
	@ConditionalOnSingleCandidate(RedisConnectionFactory.class)
	public RedisTemplate<Object, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {
		RedisTemplate<Object, Object> template = new RedisTemplate<>();
		template.setConnectionFactory(redisConnectionFactory);
		return template;
	}

	@Bean
	@ConditionalOnMissingBean
	@ConditionalOnSingleCandidate(RedisConnectionFactory.class)
	public StringRedisTemplate stringRedisTemplate(RedisConnectionFactory redisConnectionFactory) {
		StringRedisTemplate template = new StringRedisTemplate();
		template.setConnectionFactory(redisConnectionFactory);
		return template;
	}

}

~~~

### 1.1.3 @Conditional注解

从上方RedisAutoConfiguration的代码可以看到这些注解：

1. @ConditionalOnMissingBean(name = "redisTemplate")

   如果没有redisTemplate这个Bean的时候 才运行以下的代码

2. @ConditionalOnClass(RedisOperations.class)

   如果没有RedisOperations这个类（类路径上找不到，也就是没导入相关的依赖），则不运行

3. @ConditionalOnSingleCandidate(RedisConnectionFactory.class)

   指定的类存在，且必须注册在spring中，是单例的

上面的这些都是由@Conditional这个注解实现的：条件，满足条件执行，不满足条件就不执行

### 1.1.3 总结 面试题

1. @EnableAutoConfiguration 注解内部使用 @Import(AutoConfigurationImportSelector.class)来加载配置类。
2. 配置文件位置：META-INF/spring.factories，该配置文件中定义了大量的配置类，当 SpringBoot 应用启动时，会自动加载 这些配置类，初始化Bean
3. 并不是所有的Bean都会被初始化，在配置类中使用@Conditional来加载满足条件的Bean

## 1.2  自定义Starter封装

思路：

1. 创建一个maven工程，创建三个个模块
2. 一个模块为demo-app，一个模块为demo-module，一个模块为demo-module-springboot-starter
3. demo-module中定义一个类MyModule，其中有一个save方法，读取properties配置文件中的com.mszlu.version和com.mszlu.age的属性值
4. app模块 引入demo-module-springboot-starter模块，不需要初始化MyModule，只需要配置com.mszlu.version和com.mszlu.age就可以直接初始化MyModule并调用save方法

### 1.2.1 代码

1. 新建父工程

   ~~~xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>
   
       <groupId>com.mszlu</groupId>
       <artifactId>starter-parent</artifactId>
       <version>1.0-SNAPSHOT</version>
       <modules>
           <module>demo-module</module>
           <module>demo-module-springboot-starter</module>
           <module>demo-app</module>
       </modules>
       <parent>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-parent</artifactId>
           <version>2.5.0</version>
       </parent>
       <packaging>pom</packaging>
   
       <properties>
           <java.version>1.8</java.version>
       </properties>
   </project>
   ~~~

   

2. 新建demo-module，编写代码

   ~~~java
   package com.mszlu.module;
   
   public class MyModule {
   
       private String version;
   
       private Integer age;
   
       public void save(){
           System.out.println("my module save.... version:"+ version + ",age:" + age);
       }
   
       public String getVersion() {
           return version;
       }
   
       public void setVersion(String version) {
           this.version = version;
       }
   
       public Integer getAge() {
           return age;
       }
   
       public void setAge(Integer age) {
           this.age = age;
       }
   }
   
   ~~~

   

3. 新建demo-module-springboot-starter模块

   ~~~xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <parent>
           <artifactId>starter-parent</artifactId>
           <groupId>com.mszlu</groupId>
           <version>1.0-SNAPSHOT</version>
       </parent>
       <modelVersion>4.0.0</modelVersion>
   
       <artifactId>demo-module-springboot-starter</artifactId>
   
       <dependencies>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-autoconfigure</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-configuration-processor</artifactId>
               <optional>true</optional>
           </dependency>
           <dependency>
               <groupId>com.mszlu</groupId>
               <artifactId>demo-module</artifactId>
               <version>1.0-SNAPSHOT</version>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-test</artifactId>
               <scope>test</scope>
           </dependency>
       </dependencies>
   </project>
   ~~~

   ~~~java
   package com.mszlu.springbootstarter.config;
   
   import org.springframework.boot.context.properties.ConfigurationProperties;
   
   @ConfigurationProperties(prefix = "com.mszlu")
   public class ModuleConfig {
   
       private String version;
       private Integer age;
   
       public String getVersion() {
           return version;
       }
   
       public void setVersion(String version) {
           this.version = version;
       }
   
       public Integer getAge() {
           return age;
       }
   
       public void setAge(Integer age) {
           this.age = age;
       }
   }
   
   ~~~

   ```java
   package com.mszlu.springbootstarter;
   
   import com.mszlu.module.MyModule;
   import com.mszlu.springbootstarter.config.ModuleConfig;
   import org.springframework.boot.autoconfigure.condition.ConditionalOnProperty;
   import org.springframework.boot.context.properties.EnableConfigurationProperties;
   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;
   
   @Configuration
   @EnableConfigurationProperties(ModuleConfig.class)
   public class ModuleAutoConfiguration {
   
       @Bean
       @ConditionalOnProperty(name = {"com.mszlu.version","com.mszlu.age"})
       public MyModule myModule(ModuleConfig moduleConfig){
           MyModule myModule = new MyModule();
           myModule.setVersion(moduleConfig.getVersion());
           myModule.setAge(moduleConfig.getAge());
           return myModule;
       }
   }
   ```

4. 在resources下新建META-INF/spring.factories 文件

   ~~~properties
   org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
   com.mszlu.springbootstarter.ModuleAutoConfiguration
   ~~~

5. 新建demo-app模块

   ~~~xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <parent>
           <artifactId>starter-parent</artifactId>
           <groupId>com.mszlu</groupId>
           <version>1.0-SNAPSHOT</version>
       </parent>
       <modelVersion>4.0.0</modelVersion>
   
       <artifactId>demo-app</artifactId>
   
       <dependencies>
           <dependency>
               <groupId>com.mszlu</groupId>
               <artifactId>demo-module-springboot-starter</artifactId>
               <version>1.0-SNAPSHOT</version>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
       </dependencies>
   </project>
   ~~~

   

6. 编写测试类

   ~~~java
   package com.mszlu.app;
   
   import com.mszlu.module.MyModule;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   
   @SpringBootApplication
   @RestController
   @RequestMapping
   public class App {
   
   
       public static void main(String[] args) {
           SpringApplication.run(App.class,args);
       }
   
       @Autowired
       private MyModule myModule;
   
       @GetMapping
       public String hello(){
           myModule.save();
           return "success";
       }
   }
   
   ~~~

   

7. 测试

   如果没有在application.properties中配置：

   ~~~properties
   com.mszlu.version=1.0
   com.mszlu.age=20
   ~~~

   那么无法执行，如果配了可以正常执行

# 2. SpringBoot 启动流程分析

```java
SpringApplication.run(App.class,args);
```

上面的启动类，最终会调用：

~~~java
public static ConfigurableApplicationContext run(Class<?>[] primarySources, String[] args) {
		return new SpringApplication(primarySources).run(args);
	}
~~~

那么问题就变为了，new SpringApplication()的时候，做了哪些操作？

run的时候做了哪些操作？

## 2.1 new SpringApplication()

~~~java
//primarySources 启动类
public SpringApplication(Class<?>... primarySources) {
		this(null, primarySources);
	}
~~~

~~~java
public SpringApplication(ResourceLoader resourceLoader, Class<?>... primarySources) {
		this.resourceLoader = resourceLoader;
		Assert.notNull(primarySources, "PrimarySources must not be null");
    // 这可以看出来，启动类可以配置多个,放入set中去重
		this.primarySources = new LinkedHashSet<>(Arrays.asList(primarySources));
    //判断是否是一个web程序
		this.webApplicationType = WebApplicationType.deduceFromClasspath();
    //从META-INF下的spring.factories文件中获取所有BootstrapRegistryInitializer
    //初始化启动注册器，用于为ApplicationContext的创建做准备工作，运行在启动期间和环境加载之后
		this.bootstrapRegistryInitializers = getBootstrapRegistryInitializersFromSpringFactories();
    //从 spring.factories 加载所有 ApplicationContextInitializer 
    //用于在spring容器刷新之前初始化Spring ConfigurableApplicationContext的回调接口。
    //通常用于需要对应用程序上下文进行编程初始化的web应用程序中。例如，根据上下文环境注册属性源或激活配置文件等。
		setInitializers((Collection) 
   getSpringFactoriesInstances(ApplicationContextInitializer.class));
    // 从 spring.factories 加载所有 ApplicationListener
    //spring 事件监听与抽象类ApplicationEvent类配合来完成ApplicationContext的事件机制。 
		setListeners((Collection) getSpringFactoriesInstances(ApplicationListener.class));
    //推断主类
		this.mainApplicationClass = deduceMainApplicationClass();
	}
~~~



## 2.2 run()

~~~java
/**
	 * Run the Spring application, creating and refreshing a new
	 * {@link ApplicationContext}.
	 * @param args the application arguments (usually passed from a Java main method)
	 * @return a running {@link ApplicationContext}
	 */
	public ConfigurableApplicationContext run(String... args) {
        //spring的计时器 ms级别
		StopWatch stopWatch = new StopWatch();
        //计时开始
		stopWatch.start();
        //创建 DefaultBootstrapContext 会基于 bootstrapRegistryInitializers 初始化
		DefaultBootstrapContext bootstrapContext = createBootstrapContext();
		ConfigurableApplicationContext context = null;
        //属性 java.awt.headless 处理
        //Headless模式是系统的一种配置模式。在系统可能缺少显示设备、键盘或鼠标这些外设的情况下可以使用该模式。
		configureHeadlessProperty();
        //springboot启动监听器 ，可以自定义 并放入META-INF下的spring.factories中 观察spring的启动流程
        //主要是就是用来发布各种 SpringApplicationEvent
		SpringApplicationRunListeners listeners = getRunListeners(args);
        //监听开始
		listeners.starting(bootstrapContext, this.mainApplicationClass);
		try {
            //// 基于 args 构造 ApplicationArguments，即参数行命令
			ApplicationArguments applicationArguments = new DefaultApplicationArguments(args);
               /**
			 * 构造对应的 ConfigurableEnvironment 返回：
			 * 其中发布了核心事件 ApplicationEnvironmentPreparedEvent
			 * 		对应监听器完成了配置文件的读取解析
			 */
			ConfigurableEnvironment environment = prepareEnvironment(listeners, bootstrapContext, applicationArguments);
         	// spring.beaninfo.ignore 属性相关,值为“true”表示跳过对BeanInfo类的搜索
			configureIgnoreBeanInfo(environment);
           // 打印 banner 并返回 Banner 对象 彩蛋
			Banner printedBanner = printBanner(environment);
            // 根据 webApplicationType 创建对应的容器
			context = createApplicationContext();
			context.setApplicationStartup(this.applicationStartup);
            /**
			 * 在执行完所有 ApplicationContextInitializer 后
			 * 		发布事件 ApplicationContextInitializedEvent
			 * 在加载完所有资源类后发布事件 ApplicationPreparedEvent
			 */
			prepareContext(bootstrapContext, context, environment, listeners, applicationArguments, printedBanner);
            //调用容器的 refresh 方法，单例会在该阶段后创建
			refreshContext(context);
			afterRefresh(context, applicationArguments);
            //停止计时
			stopWatch.stop();
			if (this.logStartupInfo) {
				new StartupInfoLogger(this.mainApplicationClass).logStarted(getApplicationLog(), stopWatch);
			}
            //发布 ApplicationStartedEvent
			listeners.started(context);
            //执行所有 CommandLineRunner 和 ApplicationRunner
			callRunners(context, applicationArguments);
		}
		catch (Throwable ex) {
			handleRunFailure(context, ex, listeners);
			throw new IllegalStateException(ex);
		}

		try {
            //执行完所有 CommandLineRunner 和 ApplicationRunner 后，
            //发布事件 ApplicationReadyEvent
			listeners.running(context);
		}
		catch (Throwable ex) {
			handleRunFailure(context, ex, null);
			throw new IllegalStateException(ex);
		}
		return context;
	}
~~~

~~~java
package com.mszlu.app.listener;

import org.springframework.boot.ConfigurableBootstrapContext;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.SpringApplicationRunListener;
import org.springframework.context.ConfigurableApplicationContext;
import org.springframework.core.env.ConfigurableEnvironment;
import org.springframework.stereotype.Component;

public class MySpringApplicationRunListener implements SpringApplicationRunListener {

    public MySpringApplicationRunListener(SpringApplication application, String[] args) {
    }

    @Override
    public void starting(ConfigurableBootstrapContext bootstrapContext) {
        System.out.println("starting...项目启动中");
    }

    @Override
    public void environmentPrepared(ConfigurableBootstrapContext bootstrapContext,
                                    ConfigurableEnvironment environment) {
        System.out.println("environmentPrepared...环境对象开始准备");
    }

    @Override
    public void contextPrepared(ConfigurableApplicationContext context) {
        System.out.println("contextPrepared...上下文对象开始准备");
    }

    @Override
    public void contextLoaded(ConfigurableApplicationContext context) {
        System.out.println("contextLoaded...上下文对象开始加载");
    }

    @Override
    public void started(ConfigurableApplicationContext context) {
        System.out.println("started...上下文对象加载完成");
    }

    @Override
    public void running(ConfigurableApplicationContext context) {
        System.out.println("running...项目启动完成，开始运行");
    }

    @Override
    public void failed(ConfigurableApplicationContext context, Throwable exception) {
        System.out.println("failed...项目启动失败");
    }
}

~~~

~~~java
package com.mszlu.app.listener;

import org.springframework.boot.CommandLineRunner;
import org.springframework.stereotype.Component;

import java.util.Arrays;

@Component
public class MyCommandLineRunner implements CommandLineRunner {
    @Override
    public void run(String... args) throws Exception {
        System.out.println("CommandLineRunner...run");
        System.out.println(Arrays.asList(args));
    }
}

~~~

~~~java
package com.mszlu.app.listener;

import org.springframework.boot.ApplicationArguments;
import org.springframework.boot.ApplicationRunner;
import org.springframework.stereotype.Component;

import java.util.Arrays;

/**
 * 当项目启动后执行run方法。
 */
@Component
public class MyApplicationRunner implements ApplicationRunner {
    @Override
    public void run(ApplicationArguments args) throws Exception {
        System.out.println("ApplicationRunner...run");
        System.out.println(Arrays.asList(args.getSourceArgs()));
    }
}

~~~

~~~properties
org.springframework.boot.SpringApplicationRunListener=com.mszlu.app.listener.MySpringApplicationRunListener
~~~

