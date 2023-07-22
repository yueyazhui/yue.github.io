# Linux Install Ruby

通过 rvm 来安装 ruby

rvm 是 ruby 的版本管理器

```shell
yum -y install gcc curl
```

![](https://gitee.com/yueyazhui/pic-go/raw/master/img/RVM_202304051906156.png)

```shell
cd /usr/local
mkdir rvm
cd rvm/
```

查看添加到本地的密钥

```Shell
gpg2 --list-keys
```

![](https://gitee.com/yueyazhui/pic-go/raw/master/img/RVM_202304051908689.png)

添加安装 rvm 所需要的密钥

```shell
gpg2 --keyserver hkp://keyserver.ubuntu.com --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
```

![](https://gitee.com/yueyazhui/pic-go/raw/master/img/RVM_202304051911043.png)

```
gpg2 --list-keys
```

![](https://gitee.com/yueyazhui/pic-go/raw/master/img/RVM_202304051912357.png)

安装 rvm

```shell
curl -L get.rvm.io | bash -s stable
```

![](https://gitee.com/yueyazhui/pic-go/raw/master/img/RVM_202304051924914.png)

查看是否安装成功

```Shell
find / -name rvm -print
```

![](https://gitee.com/yueyazhui/pic-go/raw/master/img/RVM_202304051928695.png)

更新配置文件

```Shell
source /etc/profile.d/rvm.sh
```

下载 rvm 所需要的依赖

```shell
rvm requirements
```

![](https://gitee.com/yueyazhui/pic-go/raw/master/img/RVM_202304052002010.png)

查看 rvm 库中已知的 ruby 版本

```
rvm list known
```

![](https://gitee.com/yueyazhui/pic-go/raw/master/img/RVM_202304052008084.png)

安装需要 ruby 版本

```Shell
rvm install 2.7.2
```

![](https://gitee.com/yueyazhui/pic-go/raw/master/img/RVM_202304052054307.png)

使用指定版本的 ruby

```shell
rvm use 2.7.2
```

![](https://gitee.com/yueyazhui/pic-go/raw/master/img/RVM_202304052055247.png)

设置默认版本

```Shell
rvm use 2.7.2 --default
```

![](https://gitee.com/yueyazhui/pic-go/raw/master/img/RVM_202304052056516.png)

查看 ruby 版本

```shell
ruby -v
```

![](https://gitee.com/yueyazhui/pic-go/raw/master/img/RVM_202304052056774.png)

查看 gem 版本

```shell
gem -v
```

![](https://gitee.com/yueyazhui/pic-go/raw/master/img/RVM_202304052100920.png)

查看已安装的 ruby 版本

```shell
rvm list
```

![](https://gitee.com/yueyazhui/pic-go/raw/master/img/RVM_202304052058467.png)

换个版本

```shell
rvm install 3.0.0
```

![](https://gitee.com/yueyazhui/pic-go/raw/master/img/RVM_202304062045653.png)

```shell
rvm list
```

![](https://gitee.com/yueyazhui/pic-go/raw/master/img/RVM_202304062049718.png)

```Shell
rvm use 3.0.0
```

![](https://gitee.com/yueyazhui/pic-go/raw/master/img/RVM_202304062050781.png)

```shell
rvm list
```

![](https://gitee.com/yueyazhui/pic-go/raw/master/img/RVM_202304062054329.png)

```shell
rvm use 3.0.0 --default
```

![](https://gitee.com/yueyazhui/pic-go/raw/master/img/RVM_202304062054357.png)

```shell
rvm list
```

![](https://gitee.com/yueyazhui/pic-go/raw/master/img/RVM_202304062055253.png)

```Shell
rvm uninstall 2.7.2
```

![](https://gitee.com/yueyazhui/pic-go/raw/master/img/RVM_202304062057132.png)

```shell
rvm list
```

![](https://gitee.com/yueyazhui/pic-go/raw/master/img/RVM_202304062058007.png)

