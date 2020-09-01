# web tool



## npm

**NPM**（node package manager），通常称为node包管理器。顾名思义，它的主要功能就是管理node包，包括：安装、卸载、更新、查看、搜索、发布等。



- **安装**

  ```sh
  npm install package-name  					#本地安装
  npm install package-name @"version" #安装特定版本
  npm install -g package-name  				#全局安装
  npm install     										#通过package.json安装，将项目依赖的包都在文件内声明
  ```

  安装之前，*`npm install`*会先检查，*`node_modules`*目录之中是否已经存在指定模块。如果存在，就不再重新安装了，即使远程仓库已经有了一个新版本，也是如此。

  如果你希望，一个模块不管是否安装过，npm 都要强制重新安装，可以使用`*-f`或`*--force`参数。

  ```sh
  npm install packageName -force
  ```

- **卸载**

  ```sh
  npm uninstall package-name
  ```

- **查看已安装的包**

  ```sh
  npm ls      						#查看所有
  npm ls package-name     #查看某安装包的具体信息
  ```

- **更新**

  ```sh
  npm update package-name
  ```

  **顺序**

  先到npm模块仓库查询最新版本（提供registry查询服务）==> 返回json对象(包含该模块所有版本信息；含有dist.tarball属性，属性值为该压缩包网址，访问下载源码) ==> 查询本地版本（若本地版本不存在或远程版本较新，则安装更新）
  npm install和npm update都是以此方式安装模块

- **搜索安装包**

  ```sh
  npm seach package-name
  ```

摘自：[npm和gem](https://blog.csdn.net/u011099640/article/details/53083845)



## gem

Gem是封装起来的Ruby应用程序或代码库，是 Ruby 模块 (叫做 Gems) 的包管理器。

- **gem包安装**

  ```sh
  gem install packageName     //安装指定gem包，先从本机查找gem包并安装，若本地没有则从远程gem安装。
  gem install -l packageName      //仅从本机安装gem包
  gem install -r packageName      //仅从远程安装gem包
  gem install packageName --no-ri --no-rdoc       //安装gem包，但不安装相关文档文件
  gem install packageName --vision=[ver]      //安装指定版本的gem包12345
  ```

- **卸载gem包**

  ```sh
  gem uninstall packageName
  ```

- **查看已安装的包**

  ```sh
  gem list    //查看本机已安装的所有gem包
  gem list -r keyword     //列出远程RubyGems.org 上有此关键字的gem包（可用正则表达式）
  gem list -r>remote_gem_list.txt     //列出远程RubyGems.org 上所有Gmes清单，并保存到文件。
  gem server      //查看所有gem包文档及资料1234
  ```

- **更新安装包**

  ```sh
  gem update --system     //更新升级RubyGems软件自身
  gem update      //更新所有已安装的gem包
  gem update packageName      //更新指定的gem包
  ```

摘自：[npm和gem](https://blog.csdn.net/u011099640/article/details/53083845)