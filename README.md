# heroku 快速入门


作者 波波熊

- heroku cli安装
- heroku git开发部署
- heroku pipeline 使用


## heroku cli安装(node安装方式)

    PS C:\***> npm install -g heroku
    npm WARN deprecated cross-spawn-async@2.2.5: cross-spawn no longer requires a build toolchain, use it instead
    C:\***\npm\heroku -> C:\***\AppData\Roaming\npm\node_modules\heroku\bin\run
    + heroku@7.35.1
    added 414 packages in 20.832s
    
    
       ╭──────────────────────────────────────╮
       │                                      │
       │   Update available 5.5.1 → 6.13.4    │
       │      Run npm i -g npm to update      │
       │                                      │
       ╰──────────────────────────────────────╯
    
    > heroku --version
    heroku/7.35.1 win32-x64 node-v8.9.3
    
    PS C:\***\Desktop> heroku login -i                                                                                heroku: Enter your login credentials
    Email: ***@***.com
    Password: *******************
    Logged in as ***@***.com
    
    
##  heroku git开发部署 node.js范例


    PS E:\> mkdir testdemo                                                  
    PS E:\> cd testdemo
    PS E:\testdemo> git init
    Initialized empty Git repository in E:/testdemo/.git/
    

### 关联git 与heroku的cil

    PS E:\testdemo> heroku git:remote -a bobo-testdemo
    set git remote heroku to https://git.heroku.com/bobo-testdemo.git
    PS E:\testdemo> git remote -v
    heroku  https://git.heroku.com/bobo-testdemo.git (fetch)
    heroku  https://git.heroku.com/bobo-testdemo.git (push)
    
### 初始化node服务
    
    PS E:\testdemo> npm init -y
    Wrote to E:\testdemo\package.json:
    
    {
      "name": "testdemo",
      "version": "1.0.0",
      "description": "",
      "main": "index.js",
      "dependencies": {
        "express": "^4.17.1"
      },
      "devDependencies": {},
      "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1"
      },
      "keywords": [],
      "author": "",
      "license": "ISC"
    }
    
    PS E:\testdemo> npm install express --save
    npm WARN saveError ENOENT: no such file or directory, open 'E:\testdemo\package.json'
    npm notice created a lockfile as package-lock.json. You should commit this file.
    npm WARN enoent ENOENT: no such file or directory, open 'E:\testdemo\package.json'
    npm WARN testdemo No description
    npm WARN testdemo No repository field.
    npm WARN testdemo No README data
    npm WARN testdemo No license field.
    
    + express@4.17.1
    added 50 packages in 2.825s
    
    
### 项目路径下编写index.js文件

    let expHttp = require('express');
    let app = expHttp();
    app.get("/", (req, res) => {
        res.send("hello bo bo bear")
    });
    
    app.listen(process.env.PORT || 3000); 
    // process.env.PORT 为heroku 环境变量，负责部署时监听的端口信息
    
    
### 修改项目路径下的package.json 修改node启动文件


    {
      "name": "testdemo",
      "version": "1.0.0",
      "description": "",
      "main": "index.js",
      "dependencies": {
        "express": "^4.17.1"
      },
      "devDependencies": {},
      "scripts": {
        "start": "node index.js"
      },
      "keywords": [],
      "author": "",
      "license": "ISC"
    }

        
### 别忘记代码简洁创建.gitignore文件

将node_modules 与.idea文件不提交代码仓库
    
    node_modules/
    .idea/


### 代码提交

    PS E:\testdemo> git add .
    warning: LF will be replaced by CRLF in .idea/workspace.xml.
    The file will have its original line endings in your working directory.
    PS E:\testdemo> git commit -m "init"
    
    PS E:\testdemo> git push heroku master
    Counting objects: 5, done.
    Delta compression using up to 8 threads.
    Compressing objects: 100% (5/5), done.
    Writing objects: 100% (5/5), 646 bytes | 0 bytes/s, done.
    Total 5 (delta 3), reused 0 (delta 0)
    remote: Compressing source files... done.
    remote: Building source:
    remote:
    remote: -----> Node.js app detected
    remote:
    remote: -----> Creating runtime environment
    remote:
    remote:        NPM_CONFIG_LOGLEVEL=error
    remote:        NODE_ENV=production
    remote:        NODE_MODULES_CACHE=true
    remote:        NODE_VERBOSE=false
    remote:
    remote: -----> Installing binaries
    remote:        engines.node (package.json):  unspecified
    remote:        engines.npm (package.json):   unspecified (use default)
    remote:
    remote:        Resolving node version 12.x...
    remote:        Downloading and installing node 12.14.0...
    remote:        Using default npm version: 6.13.4
    remote:
    remote: -----> Restoring cache
    remote:        - node_modules
    remote:
    remote: -----> Installing dependencies
    remote:        Installing node modules (package.json + package-lock)
    remote:        audited 126 packages in 0.986s
    remote:        found 0 vulnerabilities
    remote:
    remote:
    remote: -----> Build
    remote:
    remote: -----> Caching build
    remote:        - node_modules
    remote:
    remote: -----> Pruning devDependencies
    remote:        audited 126 packages in 0.701s
    remote:        found 0 vulnerabilities
    remote:
    remote:
    remote: -----> Build succeeded!
    remote: -----> Discovering process types
    remote:        Procfile declares types     -> (none)
    remote:        Default types for buildpack -> web
    remote:
    remote: -----> Compressing...
    remote:        Done: 22.4M
    remote: -----> Launching...
    remote:        Released v4
    remote:        https://bobo-testdemo.herokuapp.com/ deployed to Heroku
    remote:
    remote: Verifying deploy... done.
    To https://git.heroku.com/bobo-testdemo.git
       9eb5b69..e64eb48  master -> master
       
       

### 访问日志中给出的服务地址 

    curl -k https://bobo-testdemo.herokuapp.com/
    hello bo bo bear

这就算部署成功了



## heroku pipeline 使用


### 登陆heroku 官网

https://www.heroku.com/


### 查找自己刚刚部署的应用点击进入 在deploy创建 pipeline

![微信截图_20191227172536.png](https://i.loli.net/2019/12/27/nkMtQcRewqWCE8h.png)


### 创建生产应用

![微信截图_20191227173053.png](https://i.loli.net/2019/12/27/xenoivV4rUTJyRc.png)



### 部署生产完成

![微信截图_20191227173445.png](https://i.loli.net/2019/12/27/soH2QV1Mn6zlg4C.png)