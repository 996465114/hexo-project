---
title: git 工作流
date: 2017-12-07 17:02:39
tags:
---

# GIT 工作流
## 分支介绍
* master: 存放已部署在生产环境的稳定代码，以tag标记版本节点。
* release: 发布分支，基于develop分支，用于体侧后修复bug，通常新功能不在此分支上开发。完成后合并到develp分支和master分支。
* develop: 不直接在该分支上进行开发，用来合并feature分支的代码

## git完整工作流
### 正常流
1. 接到版本需求
2. 新建featrue分支（以版本号命名分支），进入开发
3. develop完成一个小功能，需合并到本地develop分支，再push到远程分支
4. 开发完成，准备体侧
5. 禁止feature分支提交代码，完成feature分支，合并代码到develop
6. 建立release分支（以版本号命名分支），在此分支上修复测试bug
7. 测试完成，发布版本

### 异常流
1.  发现线上紧急bug
2.  从master检出release分支，在此分支上修复bug并提交测试
3.  测试完成，准备发布修复版本
4.  禁止release分支提交代码，完成release分支，将代码合并到develop分支和master分支，打上Tag

