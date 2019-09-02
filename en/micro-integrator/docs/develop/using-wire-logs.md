# Using Wire Logs

While debugging a Synapse flow, you can view the the actual HTTP
messages at the entry point of the Micro Integrator via wire logs. For
example, you can view wire logs of the incoming flow and the final
response of a proxy service. Also, you can view wire logs for points,
where it goes out from the ESB profile. For example, you can see
the outgoing and incoming wire logs for specific mediators (i.e. Call
mediator, Send mediator etc.). Wire logs are useful to troubleshoot
unexpected issues, which occurr while integrating miscallaneous systems.
You can use wire logs to verify whether the message payload is properly
going out from the server, whether the HTTP headers such as the
content-type is properly set in the outgoing message etc.
  
!!! Note
    It is recommended to enable wire logs only for troubleshooting purposes. Running productions systems with wire logs enabled is not recommended.  

## Enabling wire logs

The passthrough HTTP transport is the main transport, which handles
HTTP/HTTPS messages in the Micro Integrator. Un-comment the following entry in the
`MI_HOME/conf/log4j.properties` file to enable wire logs for the passthrough HTTP transport:

`log4j.logger.org.apache.synapse.transport.http.wire=DEBUG        `

!!! Info
    Callout mediator uses the Axis2 `         CommonsHTTPSender        ` to invoke services. It does not leverage the non-blocking NHTTP/passthrough
transports. Therefore, you need to add the following entries to the `MI_HOME/conf/log4j.properties` file to enable wire logs for the callout mediator.  
``` text
log4j.logger.httpclient.wire.header=DEBUG
log4j.logger.httpclient.wire.content=DEBUG
``` 

Following is a sample wirelog.

``` text
    [2013-09-22 19:47:57,797] DEBUG - wire >> "POST /services/StockQuoteProxy HTTP/1.1[\r][\n]"
    [2013-09-22 19:47:57,798] DEBUG - wire >> "Content-Type: text/xml; charset=UTF-8[\r][\n]"
    [2013-09-22 19:47:57,798] DEBUG - wire >> "SOAPAction: "urn:getQuote"[\r][\n]"
    [2013-09-22 19:47:57,799] DEBUG - wire >> "User-Agent: Axis2[\r][\n]"
    [2013-09-22 19:47:57,799] DEBUG - wire >> "Host: localhost:8280[\r][\n]"
    [2013-09-22 19:47:57,799] DEBUG - wire >> "Transfer-Encoding: chunked[\r][\n]"
    [2013-09-22 19:47:57,800] DEBUG - wire >> "[\r][\n]"
    [2013-09-22 19:47:57,800] DEBUG - wire >> "215[\r][\n]"
    [2013-09-22 19:47:57,800] DEBUG - wire >> "http://localhost:8280/services/StockQuoteProxyurn:uuid:9e1b0def-a24b-4fa2-8016-86cf3b458f67urn:getQuoteIBM[\r][\n]"
    [2013-09-22 19:47:57,801] DEBUG - wire >> "0[\r][\n]"
    [2013-09-22 19:47:57,801] DEBUG - wire >> "[\r][\n]"
    [2013-09-22 19:47:57,846]  INFO - TimeoutHandler This engine will expire all callbacks after : 120 seconds, irrespective of the timeout action, after the specified or optional timeout
    [2013-09-22 19:47:57,867] DEBUG - wire << "POST /services/SimpleStockQuoteService HTTP/1.1[\r][\n]"
    [2013-09-22 19:47:57,867] DEBUG - wire << "Content-Type: text/xml; charset=UTF-8[\r][\n]"
    [2013-09-22 19:47:57,867] DEBUG - wire << "SOAPAction: "urn:getQuote"[\r][\n]"
    [2013-09-22 19:47:57,867] DEBUG - wire << "Transfer-Encoding: chunked[\r][\n]"
    [2013-09-22 19:47:57,868] DEBUG - wire << "Host: localhost:9000[\r][\n]"
    [2013-09-22 19:47:57,868] DEBUG - wire << "Connection: Keep-Alive[\r][\n]"
    [2013-09-22 19:47:57,868] DEBUG - wire << "User-Agent: Synapse-PT-HttpComponents-NIO[\r][\n]"
    [2013-09-22 19:47:57,868] DEBUG - wire << "[\r][\n]"
    [2013-09-22 19:47:57,868] DEBUG - wire << "215[\r][\n]"
    [2013-09-22 19:47:57,868] DEBUG - wire << "http://localhost:8280/services/StockQuoteProxyurn:uuid:9e1b0def-a24b-4fa2-8016-86cf3b458f67urn:getQuoteIBM[\r][\n]"
    [2013-09-22 19:47:57,868] DEBUG - wire << "0[\r][\n]"
    [2013-09-22 19:47:57,869] DEBUG - wire << "[\r][\n]"
    [2013-09-22 19:47:58,002] DEBUG - wire >> "HTTP/1.1 200 OK[\r][\n]"
    [2013-09-22 19:47:58,002] DEBUG - wire >> "Content-Type: text/xml; charset=UTF-8[\r][\n]"
    [2013-09-22 19:47:58,002] DEBUG - wire >> "Date: Sun, 22 Sep 2013 14:17:57 GMT[\r][\n]"
    [2013-09-22 19:47:58,002] DEBUG - wire >> "Transfer-Encoding: chunked[\r][\n]"
    [2013-09-22 19:47:58,002] DEBUG - wire >> "Connection: Keep-Alive[\r][\n]"
    [2013-09-22 19:47:58,002] DEBUG - wire >> "[\r][\n]"
    [2013-09-22 19:47:58,014] DEBUG - wire << "HTTP/1.1 200 OK[\r][\n]"
    [2013-09-22 19:47:58,015] DEBUG - wire << "Content-Type: text/xml; charset=UTF-8[\r][\n]"
    [2013-09-22 19:47:58,015] DEBUG - wire << "Date: Sun, 22 Sep 2013 14:17:58 GMT[\r][\n]"
    [2013-09-22 19:47:58,015] DEBUG - wire << "Server: WSO2-PassThrough-HTTP[\r][\n]"
    [2013-09-22 19:47:58,016] DEBUG - wire << "Transfer-Encoding: chunked[\r][\n]"
    [2013-09-22 19:47:58,016] DEBUG - wire << "[\r][\n]"
    [2013-09-22 19:47:58,016] DEBUG - wire >> "4d8[\r][\n]"
    [2013-09-22 19:47:58,017] DEBUG - wire >> "urn:getQuoteResponseurn:uuid:9e1b0def-a24b-4fa2-8016-86cf3b458f673.827143922330303-8.819296796724336-170.50810412063595170.73218944560944Sun Sep 22 19:47:57 IST 2013-170.472077024782785.562077973231586E7IBM Company178.0616712932281324.9438904049222641.9564266653777567195.61908401976004IBM6216[\r][\n]"
    [2013-09-22 19:47:58,017] DEBUG - wire >> "0[\r][\n]"
    [2013-09-22 19:47:58,018] DEBUG - wire >> "[\r][\n]"
    [2013-09-22 19:47:58,021] DEBUG - wire << "4d8[\r][\n]"
    [2013-09-22 19:47:58,022] DEBUG - wire << "urn:getQuoteResponseurn:uuid:9e1b0def-a24b-4fa2-8016-86cf3b458f673.827143922330303-8.819296796724336-170.50810412063595170.73218944560944Sun Sep 22 19:47:57 IST 2013-170.472077024782785.562077973231586E7IBM Company178.0616712932281324.9438904049222641.9564266653777567195.61908401976004IBM6216[\r][\n]"
    [2013-09-22 19:47:58,022] DEBUG - wire << "0[\r][\n]"
    [2013-09-22 19:47:58,022] DEBUG - wire << "[\r][\n]
```

There are two incoming messages and two outgoing messages in the above
log. First part of the wire logs of a message contains the HTTP headers
and it is followed by the message payload. You need to identify the
message direction as shown below to read wire logs.

-   `           DEBUG - wire >>          ` - This represents a message,
    which is coming into WSO2 EI from the wire

-   `           DEBUG - wire <<          ` - This represents a message,
    which goes to the wire from WSO2 EI

## Viewing wire logs of a specific mediator

You need to put a debug point to the mediator, to view wire logs of
it. When debugging is finished (or while debugging), right click on the
mediator, and click **Show WireLogs** , to view wire logs for a specific
mediator.

!!! Info
    You can only view wire logs for **a whole proxy service, call mediator, send mediator, or other API resources** . However, you cannot view a
    wire log of a Synapse config (e.g. sequences), because there would not
    be anything written to wire, when the flow comes to the sequence etc.
    Hence, you can only view them in wire entry points.

![](attachments/119131423/119131424.png?effects=drop-shadow){width="400"}

## Viewing wire logs while debugging

If you view wire logs while debugging, you view only the wire logs of
mediators, whose execution is already completed as shown in the example
below.

![view wire logs while debugging](attachments/119131423/119131451.png "view wire logs while debugging"){width="800"}

## Viewing wire logs of a mediator after debugging execution

When you view wire logs of a mediator (e.g. send mediator) after
debugging, you can view the request and response wire logs as shown in
the example below.

![view wire logs of a mediator after
debugging](attachments/119131423/119131450.png "view wire logs of a mediator after debugging"){width="800"}

## Viewing wire logs of a proxy service after debugging

If you view wire logs of a proxy service after debugging finished, you
view the request wire log and final response wire log of that proxy as
shown in the example below.

![view wire logs of a proxy after
debugging](attachments/119131423/119131449.png "view wire logs of a proxy after debugging"){width="800"}  