1.nvm  一个node.js 的版本管理工具！！ npm：  nodejs 包管理工具

2. npm view webpack versions 查看webapck 历史版本

2.1 npm 常见命令
  npm list 查看 安装的包和包之间的依赖关系
  npm list | grep package_name  查看指定包之间的依赖
  npm i --production ： 只安装生产环境的包！ 因为有时候package.json中记录的包分为 开发环境和生产环境两种，测试人员的话去测试就不需要安装开发环境的包！！！

3.开发环境和生产环境

4.npm 包的版本  npm install jquery@4 -S  表示安装 jquery 4的版本中的最新版本  npm install jquery@4.1 -S   表示安装 jquery 4.1中的版本中的最新版本

- ^13.6.6

这里版本分三部分
major:16
minor:6
patch:6

符号： ^  表示 锁定 major 版本号  其他都去最新版本
      ~  表示 锁定 major和minor 两个版本好 patch取最新
      *  表示直接安装最新版本

5. 清除缓存  npm cache clean --force

6.nodejs 包 ===》 内置|插件包|自定义包

发布包到npmjs上给别人用：
1.在npm官网上注册一个账号
2 npm adduser 执行后会让你输入上面注册的账号密码
3.1 2 step ok后 执行 npm publish 即可！！ 


7 npm 脚本  在package.json 中 script属性里面编写的脚本

   "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "vue-cli-service serve",
    "runjs": "node app1.js & node app2.js" -> 执行 npm run runjs 就可以 使用执行 app1 app2 两个js脚本【这里默认两个脚本里面不涉及异步任务】 注意 一个 & 表示 这两个js脚本时并行执行的，谁先执行完不一定
    "runjs1": "node app1.js && node app2.js" -> 执行 npm run runjs1 就可以 使用执行 app1 app2 两个js脚本 注意 两个 & 表示 这两个js脚本串行执行的 一定是 app1先执行完 然后 执行 app2
  },

  npm 脚本中 读取 package.json 中的变量 
  ex: "echo": " echo $npm_package_config_env" 这里 $后面表示变量  npm_package_ 为固定写法 表示 读取package.json 中的 config 下面的env！！！ 

  8. npm 安装 git上发布的包

  cmd: npm install git+ssh://git@gitee.com:test/test.git  这里的 git@gitee.com:test/test.git 就是gitee 上面的代码仓库 ssh地址
  执行完毕 这个git仓库就会被当作是 一个包放到 node_modules 中  

  9. cross-env：  npm脚本 中当您使用 NODE_ENV=production, 来设置环境变量时，大多数 Windows 命令提示将会阻塞(报错)。（异常是Windows上的Bash，它使用本机Bash。）换言之，Windows 不支持 NODE_ENV=production 的设置方式。

    解决
    cross-env 使得您可以使用单个命令，而不必担心为平台正确设置或使用环境变量。这个迷你的包(cross-env)能够提供一个设置环境变量的 scripts，让你能够以 Unix 方式设置环境变量，然后在 Windows 上也能兼容运行。

    安装
    npm install --save-dev cross-env

    使用
    {
      "scripts": {
        "build": "cross-env NODE_ENV=production webpack --config build/webpack.config.js"
      }
    }