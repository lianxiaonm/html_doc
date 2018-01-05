> shell脚本

## brew
`安装`
```
1、安装brew
curl -LsSf http://github.com/mxcl/homebrew/tarball/master | sudo tar xvz -C/usr/local --strip 1
2、更新brew
brew update
// Error: /usr/local must be writable!
3、增加权限
sudo chown -R ${系统用户名，mac的右上角能看到} /usr/local
4、再次更新
brew update
5、按照提示删除/usr/local/share/doc/homebrew，执行
rm -r -f /usr/local/share/doc/homebrew
6、再次更新
brew update
Already up-to-date.
```
`使用`
```
```
