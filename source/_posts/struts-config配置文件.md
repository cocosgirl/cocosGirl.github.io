---
title: struts-config.xml配置文件
date: 2018-03-02 10:01:54
tags: [框架]
---

< struts-config >是struts的根元素，它主要有8个子元素，DTD定义如下17：

< !ELEMENT struts-config  
(data-sources?,form-beans?,global-exceptions?,global-forwards?,action-mappings?,controller?,message-resources\*,plug-in\*)  
\>  

以上8个元素，下面一一描述：  
1. date-sources元素  
　　date-sources元素用来配置应用程序所需要的数据源。java语言提供了javax.sql.DateSource接口，所有数据源必须实现该接口。如下配置：
>  
< data-sources>  
　　< data-source type="org.apached.commons.dbcp.BasicDataSource"\>  
　　　　………………  
　　< /data-source>  
< /data-sources>  

在Action中访问方式如下：
>>
javax.sql.DataSource dataSource;  
java.sql.Connection myConnection;  
try  
{  
　　dataSource = getDataSource(request);  
　　myConnection　＝　dataSource.getConnection();  
}  
…………  

如果是多数据源，可用如下配置：
>>
< data-sources>  
　　< data-source key="a"  type="org.apached.commons.dbcp.BasicDataSource">  
　　　　………………  
　　< /data-source>  
　　< data-source key="b"   type="org.apached.commons.dbcp.BasicDataSource">  
　　　　………………   
　　< /data-source>  
< /data-sources>

访问方式：dataSource = getDataSource(request,"a");

2.form-beans元素  
　　该元素主要用来配置表单验证的类。它包含如下属性：  
　　1.classname：一般用得少，指定和form-bean无素对应的配置类，默认为org.apache.struts.config.FormBeanConfig，如果自定义，则必须扩展FormBeanConfig类。可有可无。  
　　2.name：ActionForm Bean的惟一标识。必须。  
　　3.type：ActionForm的完整类名。必须。  
如下所示：  

>>
< form-beans>  
　　< form-bean name="Loign" type="com.ha.login"></form-bean>  
< /form-beans>

如果是动态Action FormBean，还必须配置form-bean元素的form-property子元素。它包含四个属性，上面三个，还有一个initial 元素：以字符串的形式设置表单字段的初始值，如果没有设置该属性，则是基本类型为0，如果是对象则为null。可有可无。如下所示：  

>>
< form-beans>  
　　< form-bean   
　　　　　　name="Loign"  
　　　　　　type="com.ha.login">  
　　　　　< form-property name="ok"  type="java.lang.String"/>  
　　　　　< form-property name="oks"  type="java.lang.String"/>  
　　　　　< form-property name="okss"  type="java.lang.Integer" initial="20"/>  
　　< /form-bean>  
< /form-beans>

3.global-exceptions元素  
　　该元素主要配置异常处理，它的exception子元素代表全局的异常配置。struts采取配置的方式来处理异常。它用来设置java异常和异常处理类org.apache.struts.action.ExceptionHandler之间的映射。它有七个属性，如下所示：  
　　1. className：指定和exception元素对应的配置类，默认为：org.apache.struts.config.ExceptionConfig。可有可无。  
　　2. Handler：指定异常得理类，默认为：org.apache.struts.action.ExceptionHandler。可有可无  
　　3. key：指定在Resource Bundle中描述该异常的消息key  
　　4. path：指定当异常发生时的转发路径。  
　　5. scope：指定ActionMessages实例的存放范围，可选值包括：request和session，默认为request。可有可无。  
　　6. type：指定所需处理异常类的名字，必须。  
　　7. bundle：指定Resource Bundle  
如下所示：  

>>
< global-exceptions>  
　　< exception key="global.error.invalidlogin" path="/error.jsp" scope="request" type="com.hn.tree"/>  
< /global-exceptions>

4.global-forwards元素  
　　该元素主要用来声明全局的转发关系，它具有以下四个属性：  
　　1. className：和forward元素对应的配置类，默认为：org.apache.struts.action.ActionForward。可有可无。  
　　2. contextRelative：此项为true时，表时path属性以＂/＂开头，相对于当前上下文的URL，默认为false．可有可无。　  
　　3. name：转发路径的逻辑名．必填。  
　　4. path：转发或重定向的URL，当contextRelative=false时，URL路径相对于当前应用（application），当为ture时，表示URL路径相对于当前上下文（context）。  
　　5.  redirect：当此项为ture时，表示执行重定向操作。当此项为false时表示转向操作。默认为false。  
如下所示：  
>>
< global-forwards>  
　　< forward  name="forms1"  path="/a.do"/>  
　　< forward  name="forms2"  path="/nb.jsp"/>  
< global-forwards>

5.action-mappings元素  
　　描述从特定的请求路径到相应的Action类的映射。它具有以下几个属性：  
　　1. attribute：设置和Action关联的ActionForm Bean在request和session范围内的key。如：Form Bean存在于request范围内，此项设为“myBenas”，则在request.getAttribute("myBenas")就可以返回该Bean的实例。  
　　2. classsName：和action元素对应的配置元素，默认为：org.apache.struts.action.ActionMapping.  
　　3. forward：转发的URL路径。  
　　4. include：指定包含的URL路径。  
　　5. input：输入表单的URL路径，当表单验证失败时，将把请求转发到该URL。  
　　6. name：指定和Action关联的Action FormBean的名字，该名字必须在Form-Bean定义过。  
　　7. path：指定访问Action的路径，以“/”开头，无扩展名。   
　　8. parameter：指定Actgion的配置参数，在Action类的execute()方法中，可以调用ActionMapping对象的getParameter()方法来读取该配置参数。  
　　9. roles：指定允许调用该Action的安全角色，多个角色之间用，隔开，在处理请求时，RequestProcessor会根据该配置项来决定用户是否有权限调用Action权限。  
　　10. scope：指定ActionForm Bean的存在范围，可选取为request和session，默认为session。
　　11. type：指定Action类的完整类名。  
　　12. unknown：如果此项为true，表示可以处理用户发出的所有无效的Action URL，默认为false；  
　　13. validate：指定是否要调用Action FormBean的validate方法，默认值为ture.  
注：forward、include、type属性只能选中其中一项。  
如下：  
>>
< action path="/search"  
　　　　type="zxj.okBean"  
　　　　name="a1"  
　　　　scope="request"  
　　　　validate="true"  
　　　　input="/b.jsp">  
　　< forward name="tig" path="/aa.jsp"/>  
< /action>

注：此中的forward是指局部的转发路径。global-forwards表示全局的转发路径。

6.controller元素  
　　该元素用于配置ActionServlet。它具有以下属性。  
　　1. bufferSize：指定上载文件的输入缓冲大小，可选，默认为4096  
　　2. className：指定和controller元素对应的配置类，默认为org.apache.struts.config.ControllerConfig  
　　3. conentType：字符编码，如果在Action和JSP网页中设置了，则覆盖该设置。  
　　4. locale：指定是否把Locale对象保存到当前用户的session中，默认值为false.  
　　5. processorClass：指定负责请求的java类完整路径。  
　　6. tempDir：指定处理文件的临时工作目录，如果此项没有设置，将采用Servlet容器为web应用分配的临时工作目录。  
　　7. nochache：如果为true：在响应结果中将加入特定的头参数：Pragma,Cache-Control和Expise，防止页面被保存在客户端的浏览器中，默认为false  
如下：  

>>
< controller  
　　contentType="text/html;charset="UTF-8""  
　　locale="true"  
　　processorClass="con.ok"/>

7.message-resources元素  
　　主要配置本地化消息文本，它具有以下属性。  
　　1. className：和message-resources元素对应的配置类，默认为org.apache.struts.config.MessageResourcesConfig。  
　　2. factory：指定消息资源的工厂类，默认为：org.apache.struts.util.PropertyMessageResourcesFactory类  
　　3. key：指定Resource Bundle存放的ServletContext对象中时采用的属性Key，默认由Globals.MESSAGES_KEY定义的字符串常量，只允许一个Resource Bundle采用默认的属性Key。  
　　4. null：指定MessageSources类如何处理未知消息的key，如果为true，则返回空字符串，如果为false，则返回相关字串，默认为false  
　　5. prameter：指定MessageSources的资源文件名，如果为：a.b.ApplicationResources，则实际对应的文件路径为：WEB-INF/classes/a/b/ApplicationResources.properties.  
如：  
>>
< message-resources null="false" parameter="defaultResource"/>  
< message-resources key="num1" null="false" parameter="test"/>  

访问为：
>>
< bean:message  key="zxj"/>  
< bean:message  key="zxj"  bundle="num1"/>  

其中，zxj表法，messagesource资源文件中的一个字符串。

8.plugin-in元素  
　　配置Struts的插件，属性如下：  
　　1. className：指定的Struts插件类，必须实现org.apache.struts.action.PlugiIn接口。如：  　
>>
　< plug-in className="a.b.c.">    
　　< set-property property="xxx" value="/WEB-INF/aa.xml"  />    
  < /plug-in>  
  
后记，多模块的配置，可以供多个应用应用不同的struts-config.xml

文件参考：http://blog.csdn.net/huozhicheng/article/details/5426356