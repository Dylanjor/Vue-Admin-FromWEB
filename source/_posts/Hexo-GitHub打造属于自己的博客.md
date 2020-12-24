---
title: Hexo+GitHub打造属于自己的博客
date: 2020-12-24 00:25:35
index_img: /img/bearwinnie.jpg
category: [Hexo,GitHub]
banner_img: /img/timg.jpg
tags: 
    - Hexo
    - GitHub
---
#  配置GitHub-Hexo博客的步骤

[toc]

##  1、首先配置GitHub

- __申请一个GitHub账户，然后登陆到账户__

  ![image-20201223204856625](image-20201223204856625.png)

- __创建一个与用户名一样地仓库__: __*Dylanjor.github.io*__

- __git pull origin master__ 修改点东西

- __git add . , git commit -m 'init' , git push origin master__ 

- 然后打开GitHub网站 找到 __*Setting*__ 

  ![image-20201223210447226](image-20201223210447226.png)

​       选中__*master*__分支然后点__*Save*__

- 然后访问__*https://dylanjor.github.io*__
- __成功访问__

##  2、搭配Hexo

- __Git Bash安装全局hexo:__ _npm install -g hexo_

- 新建一个空文件夹(_hexo_)__打开Git Bash__ _cd hexo_ ,  _hexo init_

- `
  $ hexo g #生成
  $ hexo s #运行
  $ hexo d #推送到GitHub远程
  $ hexo clean #清除生成的垃圾
  `

- 找一个好看的主题这里使用的是__*hexo-theme-fluid*__

- git clone https://github.com/fluid-dev/hexo-theme-fluid.git themes/fluid

- 然后将_hexo_下的__config.yml_ theme改成_fluid_与文件名一致

  ```bash
  pretty_urls:
    trailing_index: false # Set to false to remove trailing 'index.html' from permalinks
    trailing_html: true # Set to false to remove trailing '.html' from permalinks
  # 主题
  theme: hexo-theme-fluids
  deploy:
    type: git
    repo: git@github.com:Dylanjor/Dylanjor.github.io.git
    branch: master
  ```

- 执行_hexo clean,hexo g,hexo s_[预览](http://localhost:4000)

- __注意:__ 这里要下载 ```npm install hexo-deployer-git --save``` 不然会报错

- 最后没问题就可以```hexo clean,hexo g,hexo d```推送上去 [访问地址](https://dylanjor.github.io/)

##  3、Ps:上传图片 [借鉴地址](https://www.jianshu.com/p/f72aaad7b852)

- 下载插件：```npm install hexo-asset-image --save```

- 打开hexo的配置文件_config.yml

- 找到post_asset_folder，把这个选项从false改成true

- ```undefined
  找到 /node_modules/hexo-asset-image/index.js
  ```

- 将内容更换为

  ```bash
  'use strict';
  var cheerio = require('cheerio');
  
  // http://stackoverflow.com/questions/14480345/how-to-get-the-nth-occurrence-in-a-string
  function getPosition(str, m, i) {
    return str.split(m, i).join(m).length;
  }
  
  var version = String(hexo.version).split('.');
  hexo.extend.filter.register('after_post_render', function(data){
    var config = hexo.config;
    if(config.post_asset_folder){
          var link = data.permalink;
      if(version.length > 0 && Number(version[0]) == 3)
         var beginPos = getPosition(link, '/', 1) + 1;
      else
         var beginPos = getPosition(link, '/', 3) + 1;
      // In hexo 3.1.1, the permalink of "about" page is like ".../about/index.html".
      var endPos = link.lastIndexOf('/') + 1;
      link = link.substring(beginPos, endPos);
  
      var toprocess = ['excerpt', 'more', 'content'];
      for(var i = 0; i < toprocess.length; i++){
        var key = toprocess[i];
   
        var $ = cheerio.load(data[key], {
          ignoreWhitespace: false,
          xmlMode: false,
          lowerCaseTags: false,
          decodeEntities: false
        });
  
        $('img').each(function(){
          if ($(this).attr('src')){
              // For windows style path, we replace '\' to '/'.
              var src = $(this).attr('src').replace('\\', '/');
              if(!/http[s]*.*|\/\/.*/.test(src) &&
                 !/^\s*\//.test(src)) {
                // For "about" page, the first part of "src" can't be removed.
                // In addition, to support multi-level local directory.
                var linkArray = link.split('/').filter(function(elem){
                  return elem != '';
                });
                var srcArray = src.split('/').filter(function(elem){
                  return elem != '' && elem != '.';
                });
                if(srcArray.length > 1)
                  srcArray.shift();
                src = srcArray.join('/');
                $(this).attr('src', config.root + link + src);
                console.info&&console.info("update link as:-->"+config.root + link + src);
              }
          }else{
              console.info&&console.info("no src attr, skipped...");
              console.info&&console.info($(this));
          }
        });
        data[key] = $.html();
      }
    }
  });
  ```

现在就可以插入图片了，比如hexo new post photo之后
就在source/_posts生成photo.md文件和photo文件夹，我们把要插入的图片复制到photo文件夹内

```bash
![这是代替图片的文字，随便写](head.jpeg)
```


