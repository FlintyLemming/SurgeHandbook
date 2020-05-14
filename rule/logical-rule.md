# 逻辑规则

需要复杂条件时，可以用逻辑运算符去组合多个规则。并且逻辑规则是可以相互嵌套的。U

## AND 规则

当所含规则全部匹配时，会被触发

    AND,((#Rule1), (#Rule2), (#Rule3)...),Policy

例子：

    AND,((SRC-IP,192.168.1.110), (DOMAIN, example.com)),DIRECT

## OR 规则

当所含的任意一个规则匹配时，会被触发

    OR,((#Rule1), (#Rule2), (#Rule3)...),Policy

例子：

    OR,((SRC-IP,192.168.1.110), (SRC-IP,192.168.1.111)),DIRECT

## NOT 规则

当不符合所含规则时，会被触发

    NOT,((#Rule1)),Policy

例子：

    AND,((NOT,((SRC-IP,192.168.1.110))),(DOMAIN, example.com)),DIRECT

