---
title: For-ABP
date: 2020-12-20 20:53:16
tags: [ABP, .net Core]
category: C#
index_img: https://rmt.dogedoge.com/fetch/fluid/storage/bg/4xvpqo.png?w=1920&fmt=webp
banner_img: /img/timg (23).jpg
---

# VS2019问题集合

## vs2019.net core的版本太低问题

https://dotnet.microsoft.com/download/dotnet-core/3.1
![1586222467469](1586222467469.png)

数据库迁移
EF下的EF文件夹

```c#
public DbSet<UserTable> UserTable { get; set; }
```

1、Add-Migration 表名

2、update-database 

```c#
using Abp.Application.Services;
using Abp.Application.Services.Dto;
using Abp.Domain.Repositories;
using GsfCoffee.CoffeeUser;
using Microsoft.AspNetCore.Mvc;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace GsfCoffee.Coffee
{
    public class GsfInitAppService: GsfCoffeeAppServiceBase,IGsfInitAppService
    {
        private readonly IRepository<UserTable> _repository;
        public GsfInitAppService(IRepository<UserTable> repository) {
            _repository = repository;
        }

        int IGsfInitAppService.Main { get => throw new NotImplementedException(); set => throw new NotImplementedException(); }
        [HttpPost]
        /// <summary>
        /// 注册信息
        /// </summary>
        public async Task Register(UserTable _userTable)
        {
            var usertable = ObjectMapper.Map<UserTable>(_userTable);
            await _repository.InsertAsync(usertable);
        }
        /// <summary>
        /// 查询信息 登录
        /// </summary>
        /// <param name="userid">用户名</param>
        /// <param name="pwd">用户密码</param>
        /// <returns></returns>
        public async Task<ListResultDto<UserTable>> Login(string userid,string pwd)
        {
            var usertable = _repository
                .GetAll()
                .Where(c=>c.Name == userid && c.PassWord == pwd);
            return new ListResultDto<UserTable>(ObjectMapper.Map<List<UserTable>>(usertable));
        }
    }
}

    


//接口 也需要引用IApplicationService
public interface IGsfInitAppService: IApplicationService
    {
       int Main { get; set; }
    }
    
```

## VS2019 无法连接到Web服务器“IIS Express”

解决方案：删除掉项目根文件夹下的**隐藏**文件.vs和.Git

跨域问题：
![1586242794838](1586242794838.png)

目前的解决方案：
项目中的appsetting.json文件

```c#
{
  "ConnectionStrings": {
    "Default": "Server=106.15.75.186; Database=GsfCoffeeDb; User=sa; Password=Dlj0622;"
  },
  "App": {
    "ServerRootAddress": "http://localhost:21021/",
    "ClientRootAddress": "http://localhost:4200/",
    "CorsOrigins": "http://localhost:4200,http://localhost:8080,http://localhost:8081,http://localhost:3000,http://106.15.75.186:8080"
  },
  "Authentication": {
    "JwtBearer": {
      "IsEnabled": "true",
      "SecurityKey": "GsfCoffee_C421AAEE0D114E9C",
      "Issuer": "GsfCoffee",
      "Audience": "GsfCoffee"
    }
  }
}
```

将发布后的appsetting.json文件中修改
"ServerRootAddress": "http://localhost:21021/",
将"http://localhost:21021/"修改为"http://106.15.75.186:8080/"
然后网站绑定的是两个网址106.15.75.186:8080/172.17.24.86:8080
![1586315688543](1586315688543.png)

数据库创建触发器 修改数据时自动修改updatetime为当前时间

```sql
--自动修改 “修改时间”
Create TRIGGER [dbo].[update_UserTable_trigger]
 ON [dbo].[UserTable]
 after update
 as 
 declare @id int
 begin
 update UserTable set UpdateTime = SYSDATETIME() WHERE ID IN (SELECT DISTINCT ID FROM inserted)
 
 --修改numbering最大值
 Create TRIGGER [dbo].[number]
on [dbo].[UserTable]
after insert
as
declare @num int
begin
 select @num=max(numbering) from UserTable
 update UserTable set numbering=@num+1
 where ID in (select ID from inserted)
END
```

## 在iis上vs2019的Web部署

来源链接：https://www.cnblogs.com/ancm/p/11110200.html
1、服务器上下载Web Deploy
网址：https://www.iis.net/downloads/microsoft/web-deploy
.net core sdk 以及runtime 需要更新到3.1
https://dotnet.microsoft.com/download/dotnet-core
2、安装完成后
![1586594104604](1586594104604.png)

3、打开服务web management service，iis管理器权限添加上管理员的名字
4、将防火墙打开一个入站规则端口：8172在阿里云后台管理控制台上也要打开端口8172
5、右键站点-->部署-->启用web deploy发布
6、vs2019 web发布
![1586596833497](1586596833497.png)

验证连接出现对号就证明连接ok

## C#学习之路

https://mp.weixin.qq.com/s/gFWZ_txIVJX5xMAYxooR2w 作者：我亦是行人

## ABP

[toc]

* 路由：/api/TokenAuth/Authenticate
* 参数
  PassWord
  UserNameOrEmailAddress

```c#
using Microsoft.AspNetCore.Mvc;
// 忽略这个API
[ApiExplorerSettings(IgnoreApi=true)]
```

### ABP中获取Token的方法

```c#
private string CreateAccessToken(IEnumerable<Claim> claims, TimeSpan? expiration = null)
{
    var now = DateTime.UtcNow;

    var jwtSecurityToken = new JwtSecurityToken(
        issuer: _configuration.Issuer,
        audience: _configuration.Audience,
        claims: claims,
        notBefore: now,
        expires: now.Add(expiration ?? _configuration.Expiration),
        signingCredentials: _configuration.SigningCredentials
    );

    return new JwtSecurityTokenHandler().WriteToken(jwtSecurityToken);
}
private string GetEncryptedAccessToken(string accessToken)
{
    return SimpleStringCipher.Instance.Encrypt(accessToken, AppConsts.DefaultPassPhrase);
}
```

###  Core：ValidationHelper 验证

### Swagger UI显示 用户登录和注册
![abp1](abp1.png)