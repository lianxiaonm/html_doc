> [webpack](http://webpack.github.io/)是当下最热门的前端资源模块化管理和打包工具，可以参照国内的文档[webpack中文文档](http://www.css88.com/doc/webpack/)

## 基本用法

``` bash
  npm i webpack -g //全局安装
  npm i webpack --save-dev //开发本地安装
```

## webpack配置

* Entry 入口
* Output 输出
* Loaders 加载器
* Plugins 插件

``` javascript
    module.exports = {
        entry  : {
            //... string|Array<string>  --单一入口
            //... {[entryChunkName: string]: string|Array<string>} -- 多入口或分离第三库和应用入口
        },
        output : {
            filename:'',    //输出文件名。支持占位符 -- [name].[ext]
            path:'',        //输出文件的路径
        },
        resolve: {
            alias: {
                $: "jquery"
            }
        },
        target: 'node',     //构建目标
        devtool: '',        //用于错误调试更总
        module : {
            loaders: [
                {
                    test:/\.(js|jsx)$/,     //正则
                    loader:'babel'          //加载器
                }
                ...
            ]
        },
        plugins: [
            new webpack.optimize.DedupePlugin(),
            ...
        ]
    }
```

## webpack loaders
> 用于对模块的源代码进行转换，在书写的时候可以`省去`loader。例如：css-loader可以写作css。以下为常用的loader。[webpack loader list](http://webpack.github.io/docs/list-of-loaders.html)

* css-loader
* babel-loader
* url-loader
* vue-loader


## webpack插件

> webpack 支柱功能。[webpack plugin list](http://webpack.github.io/docs/list-of-plugins.html)

#### extract-text-webpack-plugin

> 提取共有css样式

* loader中使用
    ``` javascript
    {
        test  : /\.(css|less)$/,
        loader: ExtractTextPlugin.extract("style", "css!less", {
            publicPath: '../'
        })
    }
    ```

#### CommonsChunkPlugin
> 合并所有模块的共有代码

* 提取运行期的代码:
    ``` javascript
     new webpack.optimize.CommonsChunkPlugin({
         name: ["framework", "manifest"] // "manifest" 为提取运行期代码，确保公用文件缓存
     }),
    ```

#### HtmlWebpackPlugin
> 生成html文件，在webpack建构文件存在hash的时候。这个插件可以及时更新html文件中引用文件的hash

* inject    -- 注入所有的资源
    ``` javascript
    inject：'body'   //将所有JavaScript资源放置到body底部
    ```
* chunksSortMode    -- 对chuck排序
    ``` javascript
    chunksSortMode: function (chunk1, chunk2) {
        var order  = ['manifest', 'adaptive', 'plugin', 'framework', appName];
        var order1 = order.indexOf(chunk1.names[0]);
        var order2 = order.indexOf(chunk2.names[0]);
        return order1 - order2;
    }
    ```

#### HotModuleReplacementPlugin
> webpack热部署的插件

``` javascript
    devServer: {
        hot: true, // 告诉 dev-server 我们在使用 HMR
        contentBase: path.resolve(__dirname, 'dist'),
        publicPath: '/'
    }
```

#### DllPlugin和DllReferencePlugin
> 很重要的webpack插件。

#### 其他
>  * CleanWebpackPlugin -- 清除文件夹
   * ZipWebpackPlugin   --压缩文件
   * DefinePlugin       --定义源码中的全局变量值
   * UglifyJsPlugin     --混淆压缩源码



