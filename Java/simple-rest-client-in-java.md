<a
                            rel="v:url" property="v:title" class="crumbs-home" href="http://www.javacodegeeks.com/"><i
                            class="tieicon-home"></i>Home</a></span> » <span typeof="v:Breadcrumb"><a rel="v:url"
                                              property="v:title"
                                              href="http://www.javacodegeeks.com/category/java/">Java</a></span>
                        » <span typeof="v:Breadcrumb"><a rel="v:url" property="v:title"
 href="http://www.javacodegeeks.com/category/java/enterprise-java/">Enterprise
                            Java</a></span> » <span class="current">Simple REST client in Java</span>


# Simple REST client in Java

Today most of the mobile applications that used to communicate to some server use [REST](http://en.wikipedia.org/wiki/Representational_state_transfer) services. These services are also common practice to use with JavaScript or jQuery. Right now I know 2 ways to create client for REST service in java and in this article I will try to demonstrate both the ways I know hoping that it will help someone in some way.

<strong>1. Using Apache HttpClient</strong>

The Apache HttpClient library simplifies handling HTTP requests. To use this library you have to download the binaries with dependencies from [their website](http://hc.apache.org/httpclient-3.x).


Here is the code for HTTP GET method:

```java
01  import java.io.BufferedReader;
02  import java.io.IOException;
03  import java.io.InputStreamReader;
04  import org.apache.http.HttpResponse;
05  import org.apache.http.client.ClientProtocolException;
06  import org.apache.http.client.HttpClient;
07  import org.apache.http.client.methods.HttpGet;
08  import org.apache.http.impl.client.DefaultHttpClient;
09  public class Test {
10      public static void main(String[] args) throws ClientProtocolException, IOException {
11          HttpClient client = new DefaultHttpClient();
12          HttpGet request = new HttpGet('http://restUrl');
13          HttpResponse response = client.execute(request);
14          BufferedReader rd = new BufferedReader (new InputStreamReader(response.getEntity().getContent()));
15          String line = '';
16          while ((line = rd.readLine()) != null) {
17              System.out.println(line);
18          }
19      }
20  }
```

And for Post method; for sending simple string in post:

```java
01  import java.io.BufferedReader;
02  import java.io.IOException;
03  import java.io.InputStreamReader;
04  import org.apache.http.HttpResponse;
05  import org.apache.http.client.ClientProtocolException;
06  import org.apache.http.client.HttpClient;
07  import org.apache.http.client.methods.HttpPost;
08  import org.apache.http.entity.StringEntity;
09  import org.apache.http.impl.client.DefaultHttpClient;
10  public class Test {
11      public static void main(String[] args) throws ClientProtocolException, IOException {
12          HttpClient client = new DefaultHttpClient();
13          HttpPost post = new HttpPost('http://restUrl');
14          StringEntity input = new StringEntity('product');
15          post.setEntity(input);
16          HttpResponse response = client.execute(post);
17          BufferedReader rd = new BufferedReader(new InputStreamReader(response.getEntity().getContent()));
18          String line = '';
19          while ((line = rd.readLine()) != null) {
20              System.out.println(line);
21          }
22      }
23  }
```

You can also send full [JSON](http://en.wikipedia.org/wiki/JSON) or [XML](http://en.wikipedia.org/wiki/XML) of a [POJO](http://en.wikipedia.org/wiki/Plain_Old_Java_Object) by putting String representing JSON or XML as a parameter of `StringEntity` and then set the input content type. Something like this:

```java
1   StringEntity input = new StringEntity('{\'name1\':\'value1\',\'name2\':\'value2\'}'); //here instead of JSON you can also have XML
2   input.setContentType('application/json');
```                                                    

For JSON you can use JSONObject to create string representation of JSON.

```java
1   JSONObject json = new JSONObject();
2   json.put('name1', 'value1');
3   json.put('name2', 'value2');
4   StringEntity se = new StringEntity( json.toString());
```

And for sending multiple parameter in post request:

```java
01  import java.io.BufferedReader;
02  import java.io.IOException;
03  import java.io.InputStreamReader;
04  import java.util.ArrayList;
05  import java.util.List;
06  import org.apache.http.HttpResponse;
07  import org.apache.http.client.ClientProtocolException;
08  import org.apache.http.client.HttpClient;
09  import org.apache.http.client.entity.UrlEncodedFormEntity;
10  import org.apache.http.client.methods.HttpPost;
11  import org.apache.http.impl.client.DefaultHttpClient;
12  import org.apache.http.message.BasicNameValuePair;
13  public class Test {
14      public static void main(String[] args) throws ClientProtocolException, IOException {
15          HttpClient client = new DefaultHttpClient();
16          HttpPost post = new HttpPost('http://restUrl');
17          List nameValuePairs = new ArrayList(1);
18          nameValuePairs.add(new BasicNameValuePair('name', 'value'));  //you can as many name value pair as you want in the list.
19          post.setEntity(new UrlEncodedFormEntity(nameValuePairs));
20          HttpResponse response = client.execute(post);
21          BufferedReader rd = new BufferedReader(new InputStreamReader(response.getEntity().getContent()));
22          String line = '';
23          while ((line = rd.readLine()) != null) {
24              System.out.println(line);
25          }
26      }
27  }
```

<strong>2. Using Jersey</strong>

[Jersey](http://jersey.java.net/) is the reference implementation for [JSR-311](http://jcp.org/aboutJava/communityprocess/final/jsr311/index.html) specification, the specification of REST support in Java. Jersey contains basically a REST server and a REST client. it provides a library to communicate with the server producing REST services. For http get method:

                                    <div class="syntaxhighlighter  " id="highlighter_554202">
                                        <div class="bar">
                                            <div class="toolbar"><a class="item viewSource"
            style="width: 16px; height: 16px;"
            title="view source" href="#viewSource">view
                                                source</a><a class="item printSource" style="width: 16px; height: 16px;"
     title="print" href="#printSource">print</a><a
                                                    class="item about" style="width: 16px; height: 16px;" title="?"
                                                    href="#about">?</a>
                                        
                                        <div class="lines">
                                            
                                                
                                                    
                                                    
01
import 
java.io.IOException;
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
02
import 
javax.ws.rs.core.MediaType;
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
03
import 
javax.ws.rs.core.UriBuilder;
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
04
import 
org.apache.http.client.ClientProtocolException;

                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
05
import 
com.sun.jersey.api.client.Client;

                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
06
import 
com.sun.jersey.api.client.WebResource;

                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
07
import 
com.sun.jersey.api.client.config.ClientConfig;

                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
08
import 
com.sun.jersey.api.client.config.DefaultClientConfig;

                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
09
public 
class Test
    {
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
10
&nbsp;
public static
    void main(String[]
        args) throws 
    ClientProtocolException, IOException
        {
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
11

        ClientConfig config
    = new 
DefaultClientConfig();
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
12

        Client client =
    Client.create(config);
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
13

        WebResource service =
    client.resource(UriBuilder.fromUri(
        class="string">'<a
        href="http://resturl/">http://restUrl</a>'
).build());
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
14

         class="comments">// getting XML
    data
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
15

        System.out.println(service.
    path( class="string">'restPath'
).path( class="string">'resourcePath'
).accept(MediaType.APPLICATION_JSON).get(String.
class
));
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
16

         class="comments">// getting JSON
    data
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
17

        System.out.println(service.
    path( class="string">'restPath'
).path( class="string">'resourcePath'
).accept(MediaType.APPLICATION_XML).get(String.
class
));
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
18
&nbsp;
}
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
19
}
                                                    
                                                    
                                                
                                            
                                        
                                    

There are also other media formats in which you can get the response like PLAIN or HTML.

And for HTTP POST method:

                                    <div class="syntaxhighlighter  " id="highlighter_532701">
                                        <div class="bar">
                                            <div class="toolbar"><a class="item viewSource"
            style="width: 16px; height: 16px;"
            title="view source" href="#viewSource">view
                                                source</a><a class="item printSource" style="width: 16px; height: 16px;"
     title="print" href="#printSource">print</a><a
                                                    class="item about" style="width: 16px; height: 16px;" title="?"
                                                    href="#about">?</a>
                                        
                                        <div class="lines">
                                            
                                                
                                                    
                                                    
01
import 
java.io.IOException;
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
02
import 
javax.ws.rs.core.MediaType;
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
03
import 
javax.ws.rs.core.MultivaluedMap;

                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
04
import 
javax.ws.rs.core.UriBuilder;
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
05
import 
org.apache.http.client.ClientProtocolException;

                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
06
import 
com.sun.jersey.api.client.Client;

                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
07
import 
com.sun.jersey.api.client.ClientResponse;

                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
08
import 
com.sun.jersey.api.client.WebResource;

                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
09
import 
com.sun.jersey.api.client.config.ClientConfig;

                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
10
import 
com.sun.jersey.api.client.config.DefaultClientConfig;

                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
11
import 
com.sun.jersey.core.util.MultivaluedMapImpl;

                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
12
public 
class Test
    {
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
13
&nbsp;
public static
    void main(String[]
        args) throws 
    ClientProtocolException, IOException
        {
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
14

        ClientConfig config
    = new 
DefaultClientConfig();
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
15

        Client client =
    Client.create(config);
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
16

        WebResource
    webResource =
    client.resource(UriBuilder.fromUri(
        class="string">'<a
        href="http://resturl/">http://restUrl</a>'
).build());
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
17

        MultivaluedMap
    formData = new 
MultivaluedMapImpl();
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
18

        
formData.add( class="string">'name1'
, 
        class="string">'val1'
);
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
19

        
formData.add( class="string">'name2'
, 
        class="string">'val2'
);
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
20

        ClientResponse
    response =
    webResource.type(MediaType.APPLICATION_FORM_URLENCODED_TYPE).post(ClientResponse.
class,
    formData);
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
21

        
System.out.println(
        class="string">'Response ' +
    response.getEntity(String.
class
));
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
22
&nbsp;
}
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
23
}
                                                    
                                                    
                                                
                                            
                                        
                                    

If you are using your POJO in the POST then you can do something like following:

                                    <div class="syntaxhighlighter  " id="highlighter_602305">
                                        <div class="bar">
                                            <div class="toolbar"><a class="item viewSource"
            style="width: 16px; height: 16px;"
            title="view source" href="#viewSource">view
                                                source</a><a class="item printSource" style="width: 16px; height: 16px;"
     title="print" href="#printSource">print</a><a
                                                    class="item about" style="width: 16px; height: 16px;" title="?"
                                                    href="#about">?</a>
                                        
                                        <div class="lines">
                                            
                                                
                                                    
                                                    
1
ClientResponse response
    = webResource.path(
        class="string">'restPath').path(
        class="string">'resourcePath').

                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
2
type(MediaType.APPLICATION_JSON).accept(MediaType.APPLICATION_JSON).post(ClientResponse.
class,
    myPojo);
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
3

System.out.println(
        class="string">'Response ' +
    response.getEntity(String.
class
));
                                                    
                                                    
                                                
                                            
                                        
                                    

Here myPojo is an instance of custom POJO class.

You can also use Form class from Jersey to submit multiple parameters in POST request:

                                    <div class="syntaxhighlighter  " id="highlighter_243497">
                                        <div class="bar">
                                            <div class="toolbar"><a class="item viewSource"
            style="width: 16px; height: 16px;"
            title="view source" href="#viewSource">view
                                                source</a><a class="item printSource" style="width: 16px; height: 16px;"
     title="print" href="#printSource">print</a><a
                                                    class="item about" style="width: 16px; height: 16px;" title="?"
                                                    href="#about">?</a>
                                        
                                        <div class="lines">
                                            
                                                
                                                    
                                                    
01
import 
java.io.IOException;
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
02
import 
javax.ws.rs.core.MediaType;
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
03
import 
javax.ws.rs.core.UriBuilder;
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
04
import 
org.apache.http.client.ClientProtocolException;

                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
05
import 
com.sun.jersey.api.client.Client;

                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
06
import 
com.sun.jersey.api.client.ClientResponse;

                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
07
import 
com.sun.jersey.api.client.WebResource;

                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
08
import 
com.sun.jersey.api.client.config.ClientConfig;

                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
09
import 
com.sun.jersey.api.client.config.DefaultClientConfig;

                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
10
import 
com.sun.jersey.api.representation.Form;

                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
11
public 
class Test
    {
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
12
&nbsp;
public static
    void main(String[]
        args) throws 
    ClientProtocolException, IOException
        {
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
13

        ClientConfig config
    = new 
DefaultClientConfig();
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
14

        Client client =
    Client.create(config);
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
15

        WebResource service =
    client.resource(UriBuilder.fromUri(
        class="string">'<a
        href="http://resturl/">http://restUrl</a>'
).build());
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
16

        Form form
    = new 
Form();
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
17

        form.add(
        class="string">'name1'
, 
        class="string">'value1'
);
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
18

        form.add(
        class="string">'name2'
, 
        class="string">'value1'
);
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
19

        ClientResponse
    response = service.path( class="string">'restPath'
).path( class="string">'resourcePath'
).
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
20

        type(MediaType.APPLICATION_FORM_URLENCODED).post(ClientResponse.
class,
    form);
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
21

        
System.out.println(
        class="string">'Response ' +
    response.getEntity(String.
class
));
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
22
&nbsp;
}
                                                    
                                                    
                                                
                                            
                                            
                                                
                                                    
                                                    
23
}
                                                    
                                                    
                                                
                                            
                                        
                                    

Happy coding and don’t forget to share!

<strong><i>Reference: </i></strong><a href="http://harryjoy.com/2012/09/08/simple-rest-client-in-java/"
                                            rel="external nofollow" title="" class="ext-link" data-wpel-target="_blank">Simple
                                        REST client in java</a> from our <a
                                            href="http://www.javacodegeeks.com/p/jcg.html">JCG partner</a> Harsh Raval
                                        at the <a href="http://harryjoy.com/" rel="external nofollow" title=""
                                                  class="ext-link" data-wpel-target="_blank">harryjoy</a> blog.</p>
