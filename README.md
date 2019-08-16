# webpack_case
-- 创建目录
-- 安装 npm instll webpack --save-dev    3.6.0版本
-- 安装 npm install webpack-cli --save-dev
-- 创建生产目录dist与开发目录src

   - dist 中创建index.html并引入bundle.js 这是打包后的js文件
   - src 中创建entery.js 并编写代码
   - 第一次打包  webpack src/entery.js dist/bundle.js 
                webpack 入口文件 打包后存放的位置
     - 之后在dist中会生成bundle.js
     - 运行index.html查看效果

-- 创建并配置 webpack.config.js 
   - 配置 单个入口 与 单个出口 
   - 第二次打包 webpack

-- 配置多个入口与多个出口
   - 在入口处（entery）添加多个需要打包的文件  
   - 将打包后的文件名称改为 '[name].js',name是根据入口文件名称，打包成相同的名称，有几个入口文件，就可以打包出几个文件

   - 将多个文件合并在一个js中
       - 定义入口 entry:['./src/xxx.js','./src/xxx.js'],
       - 出口一个 filename:'index.js'


-- 配置文件 服务 和热更新
    - 设置 webpack-dev-server(先安装)
    - 在 devServer 中配置好之后，在packge.json中配置启动命令webpack-dev-server
    - npm run server（前期使用了最新的版本会报错，后调整为 ^2.9.7）
    - npm run server 启动后它有一种监控机制，它可以监控我们修改源码，并立即在浏览器里面给我们更新

-- css 打包
   - 创建css文件
   - style-loader 和 css-loader
       style-loader:用来处理css中url等
       css-loader:用来将css插入页面的style标签
   - 安装好了之后在module中进行配置
   - 配置好了之后重启或重新打包，不然会报错

-- 压缩js
   - 在webpack.config.js中引入 uglifyjs-webpack-plugin
   - 在plugins 中配置
   - 重新启动你会发现代码被压缩了

-- html 文件的发布（前面直接把index.html放到dist中是错误的，应该放到src目录下）
   - 将dist中index.html剪切到src目录下，并去除js引用代码，webpack会自动引用
   - 在webpack.config.js中引入 html-webpack-plugin（要先安装）
   - 接着在plugin中引入插件

-- css中图片的处理
   - 新建images目录并准备图片
   - 安装 file-loader (解决引入路径的问题) url-loader（如果图片较多，会发很多http请求，会降低页面的性能）
   - 在module中进行配置 url-loader,url-loader封装了file-loader所以暂时只配置一个就好了

-- css分离与图片路径处理
   - 安装css分离插件 extract-text-webpack-plugin
   - 安装完后用requier引用，在plugins中配置
   - 修改进行module配置 style-loader css-loader
   - 重新打包，会在dist中生成css文件，并在index目录中会有引入该css文件

   -- 图片路径问题
       - publicPath:是在webpack.config.js文件的output中，主要是用于处理静态文件路径的
       - 创建 website,在output中引用
       bug:打包后会生成绝对路径 但是会有点问题，暂时先保存原来的

-- 处理html中的图片路径
    - 把打包后的文件放到指定的文件夹下（前面的打包并没有在images文件夹） 
    - 配置 url-loader
    - 安装 html-withimg-loader 
    - 但是没看到有啥变化

-- css进阶 less文件的打包与分离
    - 安装 less
    - 安装 less-loader 用于打包使用
    - 在webpack.config.js中进行配置
    - 打包会发现head中插入了css
    -- 将css分离出来
       - 使用 extractTextPlugin
       - 在module 重新配置一下
       - 打包后会和之前的css合并在一起

-- css进阶 sass文件打包和分离
    - 安装 node-sass sass-loader 
    - node-sass:因为sass-loader依赖node-sass，所以要先安装node-sass

    - 在 module 中进行配置
    - 配置完之后出现了打包失败的问题，通过删除node包重新 install
    - 重新打包成功
    -- 将scss文件进行分离
       - 使用 extractTextPlugin
       - 在module 重新配置一下
       - 打包后会和之前的css合并在一起       

-- css进阶 自动处理css3属性前缀
    - postcss-loader 给css3属性自动添加前缀
    - 首先安装两个包 postcss-loader与autoprefixer(自动添加前缀插件)
    - 新建postcss.config.js文件,进行简单配置
    - 打包有错误，需要花时间学习一下

-- css进阶 消除未使用的css
    - 安装 purifycss-webpack purify-css
    - 引入 glob 与 purifycss-webpack
      引入完在pulgin中配置
    - 重新打包 你会发现没用的css会被过滤掉

-- 给webpack增加babel支持
    - 安装 babel-core babel-loader babel-preset-es2015 babel-preset-react

    - module中进行配置
    - .babelrc文件配置
    - 第一次打包出现了问题，发现是 babel-loader 使用最新版本有兼容问题 后改为7.1.4

    - 现在网上不流行 babel-preset-es2015 官方推荐 babel-preset-env

-- 打包后调试
    - devtool 配置

-- 实战技巧
   -- 使用模块化定义入口，通过require进行引入，然后使用
   -- 引入第三方库
      -- 在webpack.config.js中进行全局配置

   -- watch的正确使用方法
      - 当修改代码后自行打包
      - 配置watchOptions 但并没有发生什么改变

   -- 抽调第三方库（抽离出来）
      - 在入口处添加该库
      - 在plugin中进行配置

   -- 静态资源集中输出
     - 安装copy-webpack-plugin
     - 引入
     - 配置
     - 打包有错
