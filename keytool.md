1. 生成服务端私钥并导入服务端keystore文件中

```
keytool -genkey -alias serverkey -keystore server.keystore
```

2. 根据服务端私钥生成服务端公钥

```
keytool -export -alias serverkey -keystore server.keystore -file server.crt
```

3. 生成客户端私钥，并导入客户端keystore文件中

```java
keytool -genkey -alias clientKey -keystore client.keystore
```

4. 根据客户端私钥生成客户端公钥，需要用rfc方法，使导出的客户端公钥可编码

```
keytool -export -alias clientkey -keystore client.keystore -rfc -file client.crt
```

5. 把客户端公钥client.crt放置在服务端配置的trustRoot下
6. 把服务端公钥导入客户端trustkeystore中

```
keytool -import -alias serverkey -file server.crt -keystore client.keystore
```

7. 删除证书

```
keytool -delete -alias clientkey -keystore client.keystore -storepass changeit
```

8. 查看证书

```
keytool -list -v -alias clientkey -keystore client.keystore -storepass changeit
```

