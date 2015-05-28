<a
                            rel="v:url" property="v:title" class="crumbs-home" href="http://www.javacodegeeks.com/"><i
                            class="tieicon-home"></i>Home</a></span> » <span typeof="v:Breadcrumb"><a rel="v:url"
                                                                                                      property="v:title"
                                                                                                      href="http://www.javacodegeeks.com/category/java/">Java</a></span>
                        » <span typeof="v:Breadcrumb"><a rel="v:url" property="v:title"
                                                         href="http://www.javacodegeeks.com/category/java/enterprise-java/">Enterprise
                            Java</a></span> » <span class="current">Simple REST client in Java</span></div>


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

                                    <div class="syntaxhighlighter  " id="highlighter_608448">
                                        <div class="bar">
                                            <div class="toolbar"><a class="item viewSource"
                                                                    style="width: 16px; height: 16px;"
                                                                    title="view source" href="#viewSource">view
                                                source</a><a class="item printSource" style="width: 16px; height: 16px;"
                                                             title="print" href="#printSource">print</a><a
                                                    class="item about" style="width: 16px; height: 16px;" title="?"
                                                    href="#about">?</a></div>
                                        </div>
                                        <div class="lines">
                                            
                                                
                                                    
                                                    
                                                        01
                                                        import <code
                                                               >java.io.BufferedReader;
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        02
                                                        import <code
                                                               >java.io.IOException;
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        03
                                                        import <code
                                                               >java.io.InputStreamReader;
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        04
                                                        import <code
                                                               >org.apache.http.HttpResponse;
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        05
                                                        import <code
                                                               >org.apache.http.client.ClientProtocolException;
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        06
                                                        import <code
                                                               >org.apache.http.client.HttpClient;
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        07
                                                        import <code
                                                               >org.apache.http.client.methods.HttpPost;
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        08
                                                        import <code
                                                               >org.apache.http.entity.StringEntity;
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        09
                                                        import <code
                                                               >org.apache.http.impl.client.DefaultHttpClient;
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        10
                                                        public <code
                                                               >class Test
                                                            {
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        11
                                                        &nbsp;<code
                                                               >public static
                                                            void main(String[]
                                                                args) throws <code
                                                                   >ClientProtocolException, IOException
                                                                {
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        12
                                                        
                                                            &nbsp;&nbsp;HttpClient client
                                                            = new <code
                                                               >DefaultHttpClient();
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        13
                                                        
                                                            &nbsp;&nbsp;HttpPost post
                                                            = new <code
                                                               >HttpPost(<code class="string">'<a
                                                                href="http://resturl/">http://restUrl</a>'<code
                                                               >);
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        14
                                                        
                                                            &nbsp;&nbsp;StringEntity input
                                                            = new <code
                                                               >StringEntity(<code class="string">'product'<code
                                                               >);
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        15
                                                        
                                                            &nbsp;&nbsp;post.setEntity(input);
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        16
                                                        
                                                            &nbsp;&nbsp;HttpResponse response
                                                            = client.execute(post);
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        17
                                                        
                                                            &nbsp;&nbsp;BufferedReader rd
                                                            = new <code
                                                               >BufferedReader(<code
                                                               >new InputStreamReader(response.getEntity().getContent()));
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        18
                                                        
                                                            &nbsp;&nbsp;String line = <code
                                                                class="string">'';
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        19
                                                        
                                                            &nbsp;&nbsp;while <code
                                                               >((line = rd.readLine()) != <code
                                                               >null)
                                                            {
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        20
                                                        
                                                            &nbsp;&nbsp;&nbsp;System.out.println(line);
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        21
                                                        
                                                            &nbsp;&nbsp;}
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        22
                                                        &nbsp;<code
                                                               >}
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        23
                                                        }
                                                    
                                                    
                                                
                                            </div>
                                        </div>
                                    </div>

You can also send full [JSON](http://en.wikipedia.org/wiki/JSON) or [XML](http://en.wikipedia.org/wiki/XML) of a [POJO](http://en.wikipedia.org/wiki/Plain_Old_Java_Object) by putting String representing JSON or XML as a parameter of `StringEntity` and then set the input content type. Something like this:

                                    <div class="syntaxhighlighter  " id="highlighter_664644">
                                        <div class="bar">
                                            <div class="toolbar"><a class="item viewSource"
                                                                    style="width: 16px; height: 16px;"
                                                                    title="view source" href="#viewSource">view
                                                source</a><a class="item printSource" style="width: 16px; height: 16px;"
                                                             title="print" href="#printSource">print</a><a
                                                    class="item about" style="width: 16px; height: 16px;" title="?"
                                                    href="#about">?</a></div>
                                        </div>
                                        <div class="lines">
                                            
                                                
                                                    
                                                    
                                                        1
                                                        StringEntity input
                                                            = new <code
                                                               >StringEntity(<code class="string">'{\'name1\':\'value1\',\'name2\':\'value2\'}'<code
                                                               >); <code class="comments">//here
                                                            instead of JSON you can also have XML
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        2
                                                        <code
                                                               >input.setContentType(<code
                                                                class="string">'application/json'<code
                                                               >);
                                                    
                                                    
                                                
                                            </div>
                                        </div>
                                    </div>

For JSON you can use JSONObject to create string representation of JSON.

                                    <div class="syntaxhighlighter  " id="highlighter_35156">
                                        <div class="bar">
                                            <div class="toolbar"><a class="item viewSource"
                                                                    style="width: 16px; height: 16px;"
                                                                    title="view source" href="#viewSource">view
                                                source</a><a class="item printSource" style="width: 16px; height: 16px;"
                                                             title="print" href="#printSource">print</a><a
                                                    class="item about" style="width: 16px; height: 16px;" title="?"
                                                    href="#about">?</a></div>
                                        </div>
                                        <div class="lines">
                                            
                                                
                                                    
                                                    
                                                        1
                                                        JSONObject json
                                                            = new <code
                                                               >JSONObject();
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        2
                                                        json.put(<code
                                                                class="string">'name1'<code
                                                               >, <code
                                                                class="string">'value1'<code
                                                               >);
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        3
                                                        json.put(<code
                                                                class="string">'name2'<code
                                                               >, <code
                                                                class="string">'value2'<code
                                                               >);
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        4
                                                        StringEntity se
                                                            = new <code
                                                               >StringEntity( json.toString());
                                                        
                                                    
                                                    
                                                
                                            </div>
                                        </div>
                                    </div>

And for sending multiple parameter in post request:

                                    <div class="syntaxhighlighter  " id="highlighter_175563">
                                        <div class="bar">
                                            <div class="toolbar"><a class="item viewSource"
                                                                    style="width: 16px; height: 16px;"
                                                                    title="view source" href="#viewSource">view
                                                source</a><a class="item printSource" style="width: 16px; height: 16px;"
                                                             title="print" href="#printSource">print</a><a
                                                    class="item about" style="width: 16px; height: 16px;" title="?"
                                                    href="#about">?</a></div>
                                        </div>
                                        <div class="lines">
                                            
                                                
                                                    
                                                    
                                                        01
                                                        import <code
                                                               >java.io.BufferedReader;
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        02
                                                        import <code
                                                               >java.io.IOException;
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        03
                                                        import <code
                                                               >java.io.InputStreamReader;
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        04
                                                        import <code
                                                               >java.util.ArrayList;
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        05
                                                        import <code
                                                               >java.util.List;
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        06
                                                        import <code
                                                               >org.apache.http.HttpResponse;
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        07
                                                        import <code
                                                               >org.apache.http.client.ClientProtocolException;
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        08
                                                        import <code
                                                               >org.apache.http.client.HttpClient;
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        09
                                                        import <code
                                                               >org.apache.http.client.entity.UrlEncodedFormEntity;
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        10
                                                        import <code
                                                               >org.apache.http.client.methods.HttpPost;
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        11
                                                        import <code
                                                               >org.apache.http.impl.client.DefaultHttpClient;
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        12
                                                        import <code
                                                               >org.apache.http.message.BasicNameValuePair;
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        13
                                                        public <code
                                                               >class Test
                                                            {
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        14
                                                        &nbsp;<code
                                                               >public static
                                                            void main(String[]
                                                                args) throws <code
                                                                   >ClientProtocolException, IOException
                                                                {
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        15
                                                        
                                                            &nbsp;&nbsp;HttpClient client
                                                            = new <code
                                                               >DefaultHttpClient();
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        16
                                                        
                                                            &nbsp;&nbsp;HttpPost post
                                                            = new <code
                                                               >HttpPost(<code class="string">'<a
                                                                href="http://resturl/">http://restUrl</a>'<code
                                                               >);
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        17
                                                        
                                                            &nbsp;&nbsp;List nameValuePairs
                                                            = new <code
                                                               >ArrayList(<code
                                                                class="value">1);
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        18
                                                        
                                                            &nbsp;&nbsp;<code
                                                               >nameValuePairs.add(<code
                                                               >new BasicNameValuePair(<code
                                                                class="string">'name'<code
                                                               >, <code
                                                                class="string">'value'<code
                                                               >)); <code class="comments">//you
                                                            can as many name value pair as you want in the list.
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        19
                                                        
                                                            &nbsp;&nbsp;<code
                                                               >post.setEntity(<code
                                                               >new UrlEncodedFormEntity(nameValuePairs));
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        20
                                                        
                                                            &nbsp;&nbsp;HttpResponse response
                                                            = client.execute(post);
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        21
                                                        
                                                            &nbsp;&nbsp;BufferedReader rd
                                                            = new <code
                                                               >BufferedReader(<code
                                                               >new InputStreamReader(response.getEntity().getContent()));
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        22
                                                        
                                                            &nbsp;&nbsp;String line = <code
                                                                class="string">'';
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        23
                                                        
                                                            &nbsp;&nbsp;while <code
                                                               >((line = rd.readLine()) != <code
                                                               >null)
                                                            {
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        24
                                                        
                                                            &nbsp;&nbsp;&nbsp;System.out.println(line);
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        25
                                                        
                                                            &nbsp;&nbsp;}
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        26
                                                        &nbsp;<code
                                                               >}
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        27
                                                        }
                                                    
                                                    
                                                
                                            </div>
                                        </div>
                                    </div>

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
                                                    href="#about">?</a></div>
                                        </div>
                                        <div class="lines">
                                            
                                                
                                                    
                                                    
                                                        01
                                                        import <code
                                                               >java.io.IOException;
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        02
                                                        import <code
                                                               >javax.ws.rs.core.MediaType;
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        03
                                                        import <code
                                                               >javax.ws.rs.core.UriBuilder;
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        04
                                                        import <code
                                                               >org.apache.http.client.ClientProtocolException;
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        05
                                                        import <code
                                                               >com.sun.jersey.api.client.Client;
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        06
                                                        import <code
                                                               >com.sun.jersey.api.client.WebResource;
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        07
                                                        import <code
                                                               >com.sun.jersey.api.client.config.ClientConfig;
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        08
                                                        import <code
                                                               >com.sun.jersey.api.client.config.DefaultClientConfig;
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        09
                                                        public <code
                                                               >class Test
                                                            {
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        10
                                                        &nbsp;<code
                                                               >public static
                                                            void main(String[]
                                                                args) throws <code
                                                                   >ClientProtocolException, IOException
                                                                {
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        11
                                                        
                                                            &nbsp;&nbsp;ClientConfig config
                                                            = new <code
                                                               >DefaultClientConfig();
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        12
                                                        
                                                            &nbsp;&nbsp;Client client =
                                                            Client.create(config);
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        13
                                                        
                                                            &nbsp;&nbsp;WebResource service =
                                                            client.resource(UriBuilder.fromUri(<code
                                                                class="string">'<a
                                                                href="http://resturl/">http://restUrl</a>'<code
                                                               >).build());
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        14
                                                        
                                                            &nbsp;&nbsp;<code class="comments">// getting XML
                                                            data
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        15
                                                        
                                                            &nbsp;&nbsp;System.out.println(service.
                                                            path(<code class="string">'restPath'<code
                                                               >).path(<code class="string">'resourcePath'<code
                                                               >).accept(MediaType.APPLICATION_JSON).get(String.<code
                                                               >class<code
                                                               >));
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        16
                                                        
                                                            &nbsp;&nbsp;<code class="comments">// getting JSON
                                                            data
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        17
                                                        
                                                            &nbsp;&nbsp;System.out.println(service.
                                                            path(<code class="string">'restPath'<code
                                                               >).path(<code class="string">'resourcePath'<code
                                                               >).accept(MediaType.APPLICATION_XML).get(String.<code
                                                               >class<code
                                                               >));
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        18
                                                        &nbsp;<code
                                                               >}
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        19
                                                        }
                                                    
                                                    
                                                
                                            </div>
                                        </div>
                                    </div>

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
                                                    href="#about">?</a></div>
                                        </div>
                                        <div class="lines">
                                            
                                                
                                                    
                                                    
                                                        01
                                                        import <code
                                                               >java.io.IOException;
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        02
                                                        import <code
                                                               >javax.ws.rs.core.MediaType;
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        03
                                                        import <code
                                                               >javax.ws.rs.core.MultivaluedMap;
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        04
                                                        import <code
                                                               >javax.ws.rs.core.UriBuilder;
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        05
                                                        import <code
                                                               >org.apache.http.client.ClientProtocolException;
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        06
                                                        import <code
                                                               >com.sun.jersey.api.client.Client;
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        07
                                                        import <code
                                                               >com.sun.jersey.api.client.ClientResponse;
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        08
                                                        import <code
                                                               >com.sun.jersey.api.client.WebResource;
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        09
                                                        import <code
                                                               >com.sun.jersey.api.client.config.ClientConfig;
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        10
                                                        import <code
                                                               >com.sun.jersey.api.client.config.DefaultClientConfig;
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        11
                                                        import <code
                                                               >com.sun.jersey.core.util.MultivaluedMapImpl;
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        12
                                                        public <code
                                                               >class Test
                                                            {
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        13
                                                        &nbsp;<code
                                                               >public static
                                                            void main(String[]
                                                                args) throws <code
                                                                   >ClientProtocolException, IOException
                                                                {
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        14
                                                        
                                                            &nbsp;&nbsp;ClientConfig config
                                                            = new <code
                                                               >DefaultClientConfig();
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        15
                                                        
                                                            &nbsp;&nbsp;Client client =
                                                            Client.create(config);
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        16
                                                        
                                                            &nbsp;&nbsp;WebResource
                                                            webResource =
                                                            client.resource(UriBuilder.fromUri(<code
                                                                class="string">'<a
                                                                href="http://resturl/">http://restUrl</a>'<code
                                                               >).build());
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        17
                                                        
                                                            &nbsp;&nbsp;MultivaluedMap
                                                            formData = new <code
                                                               >MultivaluedMapImpl();
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        18
                                                        
                                                            &nbsp;&nbsp;<code
                                                               >formData.add(<code class="string">'name1'<code
                                                               >, <code
                                                                class="string">'val1'<code
                                                               >);
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        19
                                                        
                                                            &nbsp;&nbsp;<code
                                                               >formData.add(<code class="string">'name2'<code
                                                               >, <code
                                                                class="string">'val2'<code
                                                               >);
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        20
                                                        
                                                            &nbsp;&nbsp;ClientResponse
                                                            response =
                                                            webResource.type(MediaType.APPLICATION_FORM_URLENCODED_TYPE).post(ClientResponse.<code
                                                               >class,
                                                            formData);
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        21
                                                        
                                                            &nbsp;&nbsp;<code
                                                               >System.out.println(<code
                                                                class="string">'Response ' +
                                                            response.getEntity(String.<code
                                                               >class<code
                                                               >));
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        22
                                                        &nbsp;<code
                                                               >}
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        23
                                                        }
                                                    
                                                    
                                                
                                            </div>
                                        </div>
                                    </div>

If you are using your POJO in the POST then you can do something like following:

                                    <div class="syntaxhighlighter  " id="highlighter_602305">
                                        <div class="bar">
                                            <div class="toolbar"><a class="item viewSource"
                                                                    style="width: 16px; height: 16px;"
                                                                    title="view source" href="#viewSource">view
                                                source</a><a class="item printSource" style="width: 16px; height: 16px;"
                                                             title="print" href="#printSource">print</a><a
                                                    class="item about" style="width: 16px; height: 16px;" title="?"
                                                    href="#about">?</a></div>
                                        </div>
                                        <div class="lines">
                                            
                                                
                                                    
                                                    
                                                        1
                                                        ClientResponse response
                                                            = webResource.path(<code
                                                                class="string">'restPath').path(<code
                                                                class="string">'resourcePath').
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        2
                                                        type(MediaType.APPLICATION_JSON).accept(MediaType.APPLICATION_JSON).post(ClientResponse.<code
                                                               >class,
                                                            myPojo);
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        3
                                                        <code
                                                               >System.out.println(<code
                                                                class="string">'Response ' +
                                                            response.getEntity(String.<code
                                                               >class<code
                                                               >));
                                                    
                                                    
                                                
                                            </div>
                                        </div>
                                    </div>

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
                                                    href="#about">?</a></div>
                                        </div>
                                        <div class="lines">
                                            
                                                
                                                    
                                                    
                                                        01
                                                        import <code
                                                               >java.io.IOException;
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        02
                                                        import <code
                                                               >javax.ws.rs.core.MediaType;
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        03
                                                        import <code
                                                               >javax.ws.rs.core.UriBuilder;
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        04
                                                        import <code
                                                               >org.apache.http.client.ClientProtocolException;
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        05
                                                        import <code
                                                               >com.sun.jersey.api.client.Client;
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        06
                                                        import <code
                                                               >com.sun.jersey.api.client.ClientResponse;
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        07
                                                        import <code
                                                               >com.sun.jersey.api.client.WebResource;
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        08
                                                        import <code
                                                               >com.sun.jersey.api.client.config.ClientConfig;
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        09
                                                        import <code
                                                               >com.sun.jersey.api.client.config.DefaultClientConfig;
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        10
                                                        import <code
                                                               >com.sun.jersey.api.representation.Form;
                                                        
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        11
                                                        public <code
                                                               >class Test
                                                            {
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        12
                                                        &nbsp;<code
                                                               >public static
                                                            void main(String[]
                                                                args) throws <code
                                                                   >ClientProtocolException, IOException
                                                                {
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        13
                                                        
                                                            &nbsp;&nbsp;ClientConfig config
                                                            = new <code
                                                               >DefaultClientConfig();
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        14
                                                        
                                                            &nbsp;&nbsp;Client client =
                                                            Client.create(config);
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        15
                                                        
                                                            &nbsp;&nbsp;WebResource service =
                                                            client.resource(UriBuilder.fromUri(<code
                                                                class="string">'<a
                                                                href="http://resturl/">http://restUrl</a>'<code
                                                               >).build());
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        16
                                                        
                                                            &nbsp;&nbsp;Form form
                                                            = new <code
                                                               >Form();
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        17
                                                        
                                                            &nbsp;&nbsp;form.add(<code
                                                                class="string">'name1'<code
                                                               >, <code
                                                                class="string">'value1'<code
                                                               >);
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        18
                                                        
                                                            &nbsp;&nbsp;form.add(<code
                                                                class="string">'name2'<code
                                                               >, <code
                                                                class="string">'value1'<code
                                                               >);
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        19
                                                        
                                                            &nbsp;&nbsp;ClientResponse
                                                            response = service.path(<code class="string">'restPath'<code
                                                               >).path(<code class="string">'resourcePath'<code
                                                               >).
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        20
                                                        
                                                            &nbsp;&nbsp;type(MediaType.APPLICATION_FORM_URLENCODED).post(ClientResponse.<code
                                                               >class,
                                                            form);
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        21
                                                        
                                                            &nbsp;&nbsp;<code
                                                               >System.out.println(<code
                                                                class="string">'Response ' +
                                                            response.getEntity(String.<code
                                                               >class<code
                                                               >));
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        22
                                                        &nbsp;<code
                                                               >}
                                                    
                                                    
                                                
                                            </div>
                                            
                                                
                                                    
                                                    
                                                        23
                                                        }
                                                    
                                                    
                                                
                                            </div>
                                        </div>
                                    </div>

Happy coding and don’t forget to share!

<strong><i>Reference: </i></strong><a href="http://harryjoy.com/2012/09/08/simple-rest-client-in-java/"
                                            rel="external nofollow" title="" class="ext-link" data-wpel-target="_blank">Simple
                                        REST client in java</a> from our <a
                                            href="http://www.javacodegeeks.com/p/jcg.html">JCG partner</a> Harsh Raval
                                        at the <a href="http://harryjoy.com/" rel="external nofollow" title=""
                                                  class="ext-link" data-wpel-target="_blank">harryjoy</a> blog.</p>
