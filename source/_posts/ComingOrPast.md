---
title: ComingOrPast
date: 2020-12-20 21:17:39
tags: ForMe
category: 表结构
index_img: /img/IronMan.jpg
banner_img: /img/timg (12).jpg
---

#  ComingOrPast简介

# 数据结构

## 初始表格

### :one: ​用户表(Dlx_UserTable)

| ID   | UserName | UserPassWord | UserTrueName | UserPosition | UserLikes | UserAges | UserGender | UserMotto | UserIsDeleted  | UserLastLoginTime | CreateTime | DeletedTime | UserProfile |
| ---- | -------- | ------------ | ------------ | ------------ | --------- | -------- | ---------- | --------- | -------------- | ----------------- | ---------- | ----------- | ----------- |
| ID   | 用户名   | 用户密码     | 用户真实姓名 | 用户职位     | 用户喜好  | 用户年龄 | 用户性别   | 座右铭    | 用户是否已删除 | 最近一次登录时间  | 创建时间   | 删除时间    | 用户头像    |

### :two: 登录日志(Dlx_LoginLogTable)

| ID   | UserID | LoginTime | QuitTime | TotalTime  |
| ---- | ------ | --------- | -------- | ---------- |
| ID   | 用户ID | 登陆时间  | 退出时间 | 浏览总时长 |

### :three: 浏览记录(Dlx_BrowseRecordsTable)

| ID   | UserID | ContentID | BrowseTime | IsComment | CommentTime | IsLike   | LikeTime | IsFavorites | FavoritesTime |
| ---- | ------ | --------- | ---------- | --------- | ----------- | -------- | -------- | ----------- | ------------- |
| ID   | 用户ID | 内容ID    | 浏览时间   | 是否评论  | 评论时间    | 是否喜欢 | 喜欢时间 | 是否收藏    | 收藏时间      |

###  :four: 图片表(Dlx_PictureTable)

| ID   | ContextID | ClassID | PictureUrlOrData         | CreateTimes | CreatePerson | IsDeleted | DeletedTime |
| ---- | --------- | ------- | ------------------------ | ----------- | ------------ | --------- | ----------- |
| ID   | 内容ID    | 分类ID  | 照片地址或者是DataBase64 | 上传时间    | 上传人       | 是否删除  | 删除时间    |

### :five: 问题表(Dlx_ProblemTable)

| ID   | ProblemContext | ClassID | PictureID | CreateTime | CreatePersonID | IsDelete | DeletedTime |
| ---- | -------------- | ------- | --------- | ---------- | -------------- | -------- | ----------- |
| ID   | 问题内容       | 分类ID  | 图片ID    | 创建时间   | 创建人ID       | 是否删除 | 删除时间    |

### :six: 答案表(Dlx_AnswerTable)

| ID   | AnswerContext | ContextID | CreateTime | CreatePerson | IsDelete | DeletedTime |
| ---- | ------------- | --------- | ---------- | ------------ | -------- | ----------- |
| ID   | 答案内容      | 问题ID    | 创建时间   | 创建人       | 是否删除 | 删除时间    |

### :seven: 讨论表(Dlx_DiscussTable)

| ID   | DiscussContext | AnswerID | ProblemID | CreatePersonID | ToPersonID | CreateTime |
| ---- | -------------- | -------- | --------- | -------------- | ---------- | ---------- |
| ID   | 讨论内容       | 答案ID   | 问题ID    | 创建人ID       | 答案ID     | 创建时间   |

### :eight: 喜欢表(Dlx_LikesTable)

| ID   | LikeTypeID     | LikesContextID | CreatePersonID | CreateTime | IsDelete | DeletedTime |
| ---- | -------------- | -------------- | -------------- | ---------- | -------- | ----------- |
| ID   | 是内容还是问题 | 喜好内容       | 创建人ID       | 创建时间   | 是否删除 | 删除时间    |

### :nine: 内容表(Dlx_ContextTable)

| ID   | Context  | ClassID | PictureID | CreateTime | CreatePersonID | IsDelete | DeletedTime | IsNoComments |
| ---- | -------- | ------- | --------- | ---------- | -------------- | -------- | ----------- | ------------ |
| ID   | 问题内容 | 分类ID  | 图片ID    | 创建时间   | 创建人ID       | 是否删除 | 删除时间    | 是否禁止评论 |

### :keycap_ten:分类表(Dlx_Classification)

| ID   | IsParentClass | ParentID | ClassName | CreatePersonID | CreateTime | IsDelete | DeletedTime |
| ---- | ------------- | -------- | --------- | -------------- | ---------- | -------- | ----------- |
| ID   | 是不是父ID    | 父分类ID | 类别名称  | 创建人ID       | 创建时间   | 是否删除 | 删除时间    |

### :one::one: 评论表

| ID   | IsParentID | ParentID | CommentContext | ContextID | CreatePersonID | CreateTime | IsDelete | DeletedTime |
| ---- | ---------- | -------- | -------------- | --------- | -------------- | ---------- | -------- | ----------- |
| ID   | 是不是父ID | 父评论ID | 评论内容       | 内容ID    | 创建人ID       | 创建时间   | 是否删除 | 删除时间    |

### :one::two: 收藏表

| ID   | ContextID | FavoritesType          | CreatePersonID | CreateTime | IsDelete | DeletedTime |
| ---- | --------- | ---------------------- | -------------- | ---------- | -------- | ----------- |
| ID   | 父分类ID  | 收藏类别是内容还是问题 | 创建人ID       | 创建时间   | 是否删除 | 删除时间    |

[//]:<> "this is a comment"

### 2020/12/20
完成登录
完成注册