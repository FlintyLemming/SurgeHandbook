# 杂项规则

### **DEST-PORT**

规则会匹配相应端口的出站请求。

```
DEST-PORT,80,DIRECT
```

### **SRC-IP**

规则会匹配相应来源 IP 的入站请求。

```
SRC-IP,192.168.20.100,DIRECT
```

### **IN-PORT**

规则会匹配相应端口的入站请求。 在 Surge 监听多个端口时会有用。

```
IN-PORT,6152,DIRECT
```