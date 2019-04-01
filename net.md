# https

1. 生成keystore

```
keytool -genkey -alias tomcat -keyalg RSA
```

2. 对刚才生成的keystore自签名一下

```java
keytool -selfcert -alias tomcat -keystore .keystore
```

3. 导出证书

```java
keytool -export -alias tomcat -keystore .keystore -storepass 123456 -rfc -file tomcat.cer
```

