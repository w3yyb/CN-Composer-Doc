# 如何以编程方式安装Composer?

正如在下载页面上所指出的, 安装程序脚本包含的签名在安装程序代码更改时会发生相应改变, 因此不应长期依赖。

另一种方法是使用这个只适用于 unix 工具的脚本:

```bash
#!/bin/sh

EXPECTED_SIGNATURE=$(wget -q -O - https://composer.github.io/installer.sig)
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
ACTUAL_SIGNATURE=$(php -r "echo hash_file('SHA384', 'composer-setup.php');")

if [ "$EXPECTED_SIGNATURE" != "$ACTUAL_SIGNATURE" ]
then
    >&2 echo 'ERROR: Invalid installer signature'
    rm composer-setup.php
    exit 1
fi

php composer-setup.php --quiet
RESULT=$?
rm composer-setup.php
exit $RESULT
```

此脚本将在失败的情况下以 1退出, 或在成功时 以0退出, 如果没有错误发生, 则为安静模式.


或者, 如果您想依赖安装程序的确切副本, 您可以从 github 的历史记录中获取特定版本. 只要您可以信任 GitHub 服务器, 提交的哈希就足以使其具有唯一性和可靠性.
例如:

```bash
wget https://raw.githubusercontent.com/composer/getcomposer.org/1b137f8bf6db3e79a38a5bc45324414a6b1f9df2/web/installer -O - -q | php -- --quiet
```

您可以通过 https://github.com/composer/getcomposer.org/commits/master 上的最后一次提交哈希来替换提交哈希
