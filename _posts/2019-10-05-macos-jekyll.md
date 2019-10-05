Jekyll on macOS

# Install Command Line Tools

如果你还没有安装 Command Line Tools 你可以先安装
```
xcode-select --install
```

# Install Ruby

```
# Install Homebrew
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

brew install ruby
```
<font color="red">如果已经安装了 ruby (macOs 10.14 默认安装了 2.3.* 的版本 ruby</font>
```

Error: ruby 2.3.1_2 is already installed
To upgrade to 2.6.5, run `brew upgrade ruby`
```
那么执行
```
brew upgrade ruby
```

<font color="red">如果出现删除不掉怎么办，里面有提示</font>
```
Error: Could not remove ruby keg! Do so manually:
  sudo rm -rf /usr/local/Cellar/ruby/2.3.1_2
```

那么我们执行
```
sudo rm -rf /usr/local/Cellar/ruby/2.3.1_2
```



# Add the brew ruby path to your shell config
这里官方文档说要导入环境，因为是删除后重新安装，我们应该不需要重新导入环境变量。


# Relanuch terminal
```
which ruby
# /usr/local/opt/ruby/bin/ruby

ruby -v
ruby 2.6.3p62 (2019-04-16 revision 67580)

```

# With rbenv


# Install Jekyll

本地安装，执行Jekyll安装语句
```
gem install --user-install bundler jekyll
```
实际上本地安装我是有问题的所以
``

``