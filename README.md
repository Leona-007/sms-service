# SMS Service

一个基于 Node.js + Express + MongoDB 的学生管理后台服务端项目，主要用于学校或培训机构的用户权限管理、学校/专业/班级/学员信息管理，以及图片上传与删除等基础运营功能。

## 项目简介

该项目提供了一套完整的后台管理接口，适合用于：

- 学员信息管理
- 班级、专业、学校数据维护
- 角色与权限控制
- 用户账户管理
- 图片资源上传与存储
- 学员数据统计查询

项目前后端分离思想较明显，后端提供标准 JSON 接口，前端可直接调用这些接口完成页面交互。

---

## 技术栈

- Node.js
- Express
- MongoDB
- Mongoose
- body-parser
- multer
- blueimp-md5

---

## 项目结构

```text
sms-service/
├── app.js                 # 服务启动入口
├── package.json           # 项目依赖与脚本
├── package-lock.json      # 锁定依赖版本
├── .gitignore             # Git 忽略配置
├── models/                # Mongoose 数据模型
│   ├── UserModel.js       # 用户模型
│   ├── RoleModel.js       # 角色模型
│   ├── SchoolModel.js     # 学校模型
│   ├── MajorModel.js      # 专业模型
│   ├── ClassModel.js      # 班级模型
│   ├── StudentModel.js    # 学员模型
│   └── ...
├── public/                # 静态资源目录
│   └── upload/            # 图片上传保存目录
├── routers/
│   ├── index.js           # 主路由，包含大部分业务接口
│   ├── file-upload.js     # 文件上传与删除接口
│   └── test.html
└── node_modules/          # 依赖安装目录
```

---

## 主要功能

### 1. 用户认证与权限

- 登录接口
- 获取角色列表
- 添加角色
- 设置角色权限
- 获取当前用户权限菜单
- 修改密码
- 校验原密码

### 2. 用户管理

- 用户列表查询（分页）
- 用户新增
- 用户详情查询
- 用户更新
- 用户删除
- 默认超管账户初始化

### 3. 学校/专业/班级管理

- 学校列表、增加、查询、更新、删除
- 专业列表、增加、查询、更新、删除
- 班级列表、增加、查询、更新、删除
- 支持基于筛选条件查询

### 4. 学员管理

- 学员列表查询（分页、条件筛选）
- 学员新增
- 学员信息更新
- 学员删除
- 学员详情查询
- 按入学年份统计学生数据

### 5. 文件上传

- 图片上传接口
- 图片删除接口
- 上传文件保存在 public/upload 目录

---

## 默认账号

系统在启动时会自动初始化一个超级管理员账号：

- 用户名：admin
- 密码：admin

> 该账号用于后台管理初始登录，实际部署时建议修改为更安全的密码。

---

## 数据库配置

项目默认连接 MongoDB 数据库：

```js
mongodb://localhost/b0433stu
```

启动前请确保本地 MongoDB 已安装并运行正常，且可访问该数据库。

如果数据库未启动，应用会在控制台输出连接失败信息，服务不会正常提供接口。

---

## 安装与运行

### 1. 安装依赖

```bash
npm install
```

### 2. 启动 MongoDB

确保本地 MongoDB 服务已启动。

### 3. 启动项目

```bash
node app.js
```

启动成功后，服务会监听：

```text
http://localhost:3000
```

---

## 接口说明

项目的主接口集中在 routers/index.js，统一以 JSON 形式返回数据，常见返回格式如下：

```json
{
  "status": 0,
  "data": {}
}
```

或者：

```json
{
  "status": 1,
  "msg": "错误信息"
}
```

### 用户相关

- POST /login
  - 用户登录
- GET /manage/role/list
  - 获取角色列表
- POST /manage/role/add
  - 新增角色
- POST /manage/role/update
  - 更新角色权限
- GET /manage/user/all
  - 获取所有用户
- POST /manage/user/list
  - 分页获取用户列表
- POST /manage/user/add
  - 添加用户
- GET /manage/user/find
  - 根据 ID 查询用户
- POST /manage/user/update
  - 修改用户信息
- POST /manage/user/delete
  - 删除用户
- POST /menus
  - 获取用户权限菜单
- POST /manage/user/pwd
  - 校验原密码
- PUT /manage/user/pwd
  - 修改密码

### 学校/专业/班级/学员

- POST /manage/school/list
- POST /manage/school/add
- GET /manage/school/find
- POST /manage/school/update
- POST /manage/school/delete
- GET /manage/school/all
- POST /manage/major/list
- POST /manage/major/add
- GET /manage/major/find
- POST /manage/major/update
- POST /manage/major/delete
- GET /manage/major/all
- GET /manage/class/all
- POST /manage/class/list
- POST /manage/class/add
- GET /manage/class/find
- POST /manage/class/update
- POST /manage/class/delete
- POST /manage/student/list
- POST /manage/student/add
- GET /manage/student/find
- POST /manage/student/update
- POST /manage/student/delete
- POST /manage/student/date
  - 按年份统计入学人数

### 文件上传

- POST /manage/img/upload
  - 上传图片
- POST /manage/img/delete
  - 删除图片

---

## 图片上传说明

上传文件处理逻辑位于 routers/file-upload.js。

- 上传目录：public/upload
- 文件命名方式：fieldname + 时间戳 + 扩展名
- 返回 URL：

```text
http://localhost:3000/upload/文件名
```

---

## 注意事项

1. 本项目是后端服务，不包含前端页面源码。
2. 需要先启动 MongoDB，否则服务无法连接数据库。
3. 默认管理员账号密码较简单，实际部署前请修改为强密码。
4. 生产环境建议增加环境变量配置、JWT 鉴权、日志系统和更完善的异常处理。

---

## 适用场景

- 教务管理平台后端
- 学校学生信息系统
- 培训机构管理平台
- 角色权限型后台接口服务

---

## 许可证

该项目使用 ISC License。

---

## 维护建议

在真实生产环境中，建议进一步完善以下能力：

- 使用环境变量管理数据库连接和端口配置
- 增加统一鉴权中间件
- 统一 API 错误处理
- 增加日志与监控
- 使用 Redis 或缓存提升接口性能
- 增加单元测试与接口测试

---

## 总结

本项目是一个功能较完整的学生管理后台后端模板，覆盖了用户、角色、学校、专业、班级、学员和图片上传等核心业务。它适合用于二次开发和教学演示，也可作为中小型学校或培训机构管理系统的基础后端实现。
