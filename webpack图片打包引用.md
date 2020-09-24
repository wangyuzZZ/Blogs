# webpack3 CSS & JS引用图片问题

## 文件目录

```javascript
build
dist
	static
 	···
 	image
 	···
 	index.html
node_moudles
src
	images
    js
    less
    ...
    index.html
package.json
readme.md
yarn.lock

//webpack loader 处理图片及less文件配置
module.exports = {
    rules: [
        //处理css
        {
            test: /.less$/,
            exclude: /node_modules/,
            use: ExtractTextPlugin.extract({
                fallback: 'style-loader',
                use:["css-loader",{
                    loader:"postcss-loader",
                    options: {
                        ident: 'postcss',
                        plugins: [
                            require('autoprefixer')({
                                browsers: ["last 2 versions"]
                            }),
                        ]
                    }
                },"less-loader"]
            })
        },
       // 处理图片
       {
        test: /\.(png|jpg|jpeg|gif)$/,
        exclude: /node_modules/,
        use: {
            loader: "url-loader",
            options: {
                limit: "1024",
                name: "[name].[ext]",
                outputPath: "static/images/"
            }
        }
    }
        ...
};
//less 文件中正确引用方式
background: url("/src/images/bj-3.png") no-repeat; //错误（build之后不会打包图片）
background: url("../images/bj-3.png") no-repeat; //正确
//JS 中引用图片
//静态引用
path = require('../../images/verify-img/bj-3.png')//正确
//动态引用
let url = 'bj-3'
path = equire('../../images/verify-img/' + url + '.png')//正确
//错误示例
let url = '../../images/verify-img/bj-3'
path = equire(url + '.png')
```

>`require` 不接受变量的。