# ws���Թ����ࣨJava��

package test;                <br>

import java.io.ByteArrayInputStream;                <br>
import java.io.ByteArrayOutputStream;                <br>
import java.io.IOException;                <br>
import java.net.URI;                <br>
import java.nio.ByteBuffer;                <br>
import java.nio.CharBuffer;                <br>
import java.nio.charset.Charset;                <br>
import java.nio.charset.CharsetDecoder;                <br>
import java.security.cert.CertificateException;                <br>
import java.security.cert.X509Certificate;                <br>
import java.util.HashMap;                <br>
import java.util.Map;                <br>
import java.util.zip.GZIPInputStream;                <br>

import javax.net.ssl.SSLContext;                <br>
import javax.net.ssl.TrustManager;                <br>
import javax.net.ssl.X509TrustManager;                <br>

import org.java_websocket.client.DefaultSSLWebSocketClientFactory;                <br>
import org.java_websocket.client.WebSocketClient;                <br>
import org.java_websocket.drafts.Draft;                <br>
import org.java_websocket.drafts.Draft_17;                <br>
import org.java_websocket.handshake.ServerHandshake;                <br>

/**                <br>
\* @author ����� DateTime:2018��11��22�� ����9:25:20                 <br>
\* ����ʹ�õ�websocket client�汾                <br>
\* \<dependency\>                <br>
\* \<groupId\>org.java-websocket\</groupId\>                <br>
\* \<artifactId\>Java-WebSocket\</artifactId\>                <br>
\* \<version\>1.3.0\</version\>                <br>
\* \</dependency\>                <br>
\*                 <br>
*/                <br>
public class WsTest {                <br>

public static void main(String[] args) {                <br>
try {                <br>
//wsurl                <br>
String url = "wss://ws.hologo.io/kline-api/ws";                <br>
//��ʷ�����������                <br>
String reqParam = "{\"event\":\"req\",\"params\":{\"channel\":\"market_btcusdt_trade_ticker\",\"cb_id\":\"btcusdt\",\"top\":150}}";                <br>
//���Ĳ���                <br>
String subParam = "{\"event\":\"sub\",\"params\":{\"channel\":\"market_btcusdt_trade_ticker\",\"cb_id\":\"btcusdt\",\"top\":150}}";                <br>

//��ʼ��������ʷ����                <br>
WebSocketUtils wsc = WebSocketUtils.executeWebSocket(url, reqParam);                <br>

//����ʵʱ����                <br>
wsc.send(subParam);                <br>

//�̲߳��������ȴ��µ���Ϣ��www.hologo.io/ һ��һ�������һ����µĳɽ�����                <br>
while (true) {                <br>
Thread.sleep(1000);                <br>
}                <br>

}catch (Exception e) {                <br>
e.printStackTrace();                <br>
}                <br>
}                <br>

static class WebSocketUtils extends WebSocketClient {                <br>
private static WebSocketUtils wsclient = null;                <br>
private String msg = "";                <br>

public WebSocketUtils(URI serverURI) {                <br>
super(serverURI);                <br>
}                <br>

public WebSocketUtils(URI serverUri, Draft draft) {                <br>
super(serverUri, draft);                <br>
}                <br>

public WebSocketUtils(URI serverUri, Map<String, String> headers, int connecttimeout) {                <br>
super(serverUri, new Draft_17(), headers, connecttimeout);                <br>
}                <br>

@Override                <br>
public void onOpen(ServerHandshake serverHandshake) {                <br>
System.out.println("�����ѽ���");                <br>
                <br>
}                <br>

@Override                <br>
public void onMessage(String s) {                <br>
System.out.println("�յ��ַ�����Ϣ");                <br>
}                <br>

@Override                <br>
public void onClose(int i, String s, boolean b) {                <br>
System.out.println("�����ѹر�");                <br>
}                <br>

@Override                <br>
public void onError(Exception e) {                <br>
System.out.println("������");                <br>
}                <br>

@Override                <br>
public void onMessage(ByteBuffer socketBuffer) {                <br>
try {                <br>
String marketStr = byteBufferToString(socketBuffer);                <br>
String market = uncompress(marketStr).toLowerCase();                <br>
if (market.contains("ping")) {                <br>
System.out.println("�յ���Ϣping��"+market);                <br>
String tmp = market.replace("ping", "pong");                <br>
wsclient.send(market.replace("ping", "pong"));                <br>
} else {                <br>
msg = market;                <br>
System.out.println("�յ���Ϣ��"+msg);                <br>
}                <br>
} catch (IOException e) {                <br>
e.printStackTrace();                <br>
}                <br>
}                <br>

public static Map<String, String> getWebSocketHeaders() throws IOException {                <br>
Map<String, String> headers = new HashMap<String, String>();                <br>
return headers;                <br>
}                <br>

private static void trustAllHosts(WebSocketUtils appClient) {                <br>
TrustManager[] trustAllCerts = new TrustManager[] { new X509TrustManager() {                <br>
public java.security.cert.X509Certificate[] getAcceptedIssuers() {                <br>
return new java.security.cert.X509Certificate[] {};                <br>
}                <br>

public void checkClientTrusted(X509Certificate[] chain, String authType) throws CertificateException {                <br>
}                <br>

public void checkServerTrusted(X509Certificate[] chain, String authType) throws CertificateException {                <br>
}                <br>
} };                <br>

try {                <br>
SSLContext sc = SSLContext.getInstance("TLS");                <br>
sc.init(null, trustAllCerts, new java.security.SecureRandom());                <br>
appClient.setWebSocketFactory(new DefaultSSLWebSocketClientFactory(sc));                <br>
} catch (Exception e) {                <br>
e.printStackTrace();                <br>
}                <br>
}                <br>

public static WebSocketUtils executeWebSocket(String url,String sendMsg) throws Exception {                <br>
wsclient = new WebSocketUtils(new URI(url), getWebSocketHeaders(), 1000);                <br>
trustAllHosts(wsclient);                <br>
wsclient.connectBlocking();                <br>
wsclient.send(sendMsg);                <br>
return wsclient;                <br>
}                <br>

// buffer תString                <br>
public String byteBufferToString(ByteBuffer buffer) {                <br>
CharBuffer charBuffer = null;                <br>
try {                <br>
Charset charset = Charset.forName("ISO-8859-1");                <br>
CharsetDecoder decoder = charset.newDecoder();                <br>
charBuffer = decoder.decode(buffer);                <br>
buffer.flip();                <br>
return charBuffer.toString();                <br>
} catch (Exception ex) {                <br>
ex.printStackTrace();                <br>
return null;                <br>
}                <br>
}                <br>


// ��ѹ��                <br>
public String uncompress(String str) throws IOException {                <br>
if (str == null || str.length() == 0) {                <br>
return str;                <br>
}                <br>
ByteArrayOutputStream out = new ByteArrayOutputStream();                <br>
ByteArrayInputStream in = new ByteArrayInputStream(str.getBytes("ISO-8859-1"));                <br>
GZIPInputStream gunzip = new GZIPInputStream(in);                <br>
byte[] buffer = new byte[256];                <br>
int n;                <br>
while ((n = gunzip.read(buffer)) >= 0) {                <br>
out.write(buffer, 0, n);                <br>
}                <br>
return out.toString();                <br>
}                <br>

}                <br>
}