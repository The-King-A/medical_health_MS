# 健康咨询系统 (Health Consultant System)

基于Spring Boot 3.x + MyBatis + MySQL的医疗健康管理系统。

## 项目特点

- **技术栈**：Spring Boot 3.2.10 + Java 17 + MyBatis 3.0.5 + MySQL
- **注解优先**：优先使用MyBatis注解，减少XML配置
- **完整架构**：包含实体层、数据访问层、业务层、控制层
- **统一响应**：标准化API响应格式
- **事务管理**：基于Spring的声明式事务
- **全局异常处理**：统一的异常处理机制

## 功能模块

### 1. 护理管理模块

- **护理级别管理**：护理级别的增删改查
- **护理内容管理**：护理项目的管理
- **级别内容关联**：护理级别与内容的多对多关系管理
- **护理记录管理**：护理执行记录的管理

### 2. 客户管理模块

- **客户信息管理**：客户基本信息的CRUD操作
- **身份验证**：身份证号和联系方式唯一性验证
- **条件查询**：支持多条件动态查询和分页

### 3. 入住管理模块

- **入住登记**：客户入住信息登记
- **退住管理**：客户退住信息管理
- **外出登记**：客户外出信息管理
- **状态查询**：入住状态和房间占用状态查询

### 4. 用药管理模块

- **用药记录**：药品使用记录管理
- **冲突检测**：用药时间冲突检测
- **当前用药**：查询客户当前正在使用的药品

## 数据库表结构

### 护理模块

- `nurselevel` - 护理级别表
- `nursecontent` - 护理内容表
- `nurselevelitem` - 护理级别内容关联表
- `nursing_record` - 护理记录表

### 客户管理模块

- `customer` - 客户信息表
- `check_in` - 入住登记表
- `check_out` - 退住登记表
- `outgoing` - 外出登记表

### 用药管理模块

- `medication` - 用药管理表

## 快速开始

### 1. 环境要求

- Java 17+
- MySQL 8.0+
- Maven 3.6+

### 2. 数据库配置

1. 创建数据库 `medical_health_system`
2. 修改 `application.yml` 中的数据库连接信息

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/medical_health_system
    username: your_username
    password: your_password
```

### 3. 运行项目

```bash
# 克隆项目
git clone [项目地址]

# 进入项目目录
cd health_consultant_system

# 编译打包
mvn clean compile

# 运行项目
mvn spring-boot:run
```

### 4. 访问系统

- 系统地址：http://localhost:8080
- 健康检查：http://localhost:8080/actuator/health

## API接口文档

### 客户管理 API

- `GET /api/customers` - 查询所有客户
- `GET /api/customers/{id}` - 根据ID查询客户
- `GET /api/customers/page` - 分页查询客户
- `POST /api/customers` - 新增客户
- `PUT /api/customers/{id}` - 更新客户信息
- `DELETE /api/customers/{id}` - 删除客户

### 护理管理 API

- `GET /api/nursing/levels` - 查询护理级别
- `GET /api/nursing/contents` - 查询护理内容
- `GET /api/nursing/records` - 查询护理记录
- `POST /api/nursing/levels` - 新增护理级别
- `POST /api/nursing/contents` - 新增护理内容
- `POST /api/nursing/records` - 新增护理记录

### 入住管理 API

- `GET /api/checkinout/checkins` - 查询入住记录
- `GET /api/checkinout/checkouts` - 查询退住记录
- `GET /api/checkinout/outgoings` - 查询外出记录
- `POST /api/checkinout/checkins` - 入住登记
- `POST /api/checkinout/checkouts` - 退住登记
- `POST /api/checkinout/outgoings` - 外出登记

### 用药管理 API

- `GET /api/medications` - 查询用药记录
- `GET /api/medications/customer/{customerId}` - 查询客户用药记录
- `GET /api/medications/active` - 查询当前有效用药
- `POST /api/medications` - 新增用药记录
- `PUT /api/medications/{id}` - 更新用药记录
- `PUT /api/medications/{id}/stop` - 停止用药

## 统一响应格式

```json
{
  "code": 200,
  "message": "操作成功",
  "data": {},
  "timestamp": 1640995200000
}
```

## 技术实现特点

### MyBatis注解优先策略

- 简单CRUD操作使用 `@Select`、`@Insert`、`@Update`、`@Delete` 注解
- 复杂动态SQL使用 `@SelectProvider`、`@InsertProvider` 等Provider注解
- 主键回填使用 `@Options(useGeneratedKeys = true, keyProperty = "id")`
- 结果映射启用驼峰命名自动映射

### 事务管理

- Service层方法使用 `@Transactional` 注解
- 支持声明式事务管理
- 异常时自动回滚

### 全局异常处理

- 统一的异常处理机制
- 标准化错误响应格式
- 详细的日志记录
