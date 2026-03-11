---
name: java-teacher
description: Java 教师讲解 Java 与后端技术时使用的教学要点。当用户学习 Java、阅读 scp-foundation 代码、按 learn 目录提问时引用本 skill。
---

# Java 教师 Skill

教师 agent 在讲解时按本 skill 组织内容，确保与 learn 目录、scp-foundation 对齐。

---

## 1. 学习目录与路径

- **学习计划**：`learn/Java与后端技术学习计划.md`
- **总目录**：`learn/README.md`
- **阶段目录**：`learn/01-Java基础/`、`learn/02-Maven与项目结构/` … `learn/07-进阶主题/`
- **主题目录**：每个阶段下的子目录（如 `01-语法基础`、`02-面向对象`），内有 README 说明「需要看懂的内容」和「在 scp 中怎么找」。

---

## 2. scp-foundation 快速查找

| 想看懂什么 | 在 scp 中怎么找 |
|------------|-----------------|
| REST 接口 | `**/*Controller.java`，搜索 `@PostMapping`、`@GetMapping` |
| 业务逻辑 | `**/*ServiceImpl.java`，搜索 `@Service` |
| 数据访问 | `**/*Mapper.java`、`**/mapper/**/*.xml` |
| 异常抛出 | 搜索 `BusinessException`、`BusinessErrorEnum` |
| DTO/VO | `**/api/**/*DTO.java`、`**/*VO.java` |
| 配置 | `**/application*.yml`、`**/bootstrap.yml` |
| Feign 调用 | 搜索 `@FeignClient`、`*Feign.java`、`*ClientImpl.java` |

---

## 3. 讲解要点（按阶段）

### 阶段一：Java 基础
- 语法：变量、类型、if/for、数组 → 任意 Service 方法体
- 面向对象：类、继承、接口 → DTO、VO、Entity 类
- 集合与 Stream：List、Map、stream()、collect() → 搜索 `stream()`
- 异常：try-catch、throw → 搜索 `BusinessException`
- Lombok：@Data、@Getter、@Slf4j → 所有 DTO/VO

### 阶段二：Maven 与项目结构
- 模块：api / domain / sdk / service 职责 → `pom.xml` 的 `<modules>`
- 依赖：谁依赖谁 → 各模块 `pom.xml` 的 `<dependencies>`

### 阶段三：Spring Boot
- IoC/DI：@Service、@Resource → Controller 的成员变量
- Web：@RestController、@PostMapping、@RequestBody → `*Controller.java`
- 统一响应：BaseResponse → Controller 返回值
- 事务：@Transactional → Service 方法上

### 阶段四：分层与规范
- 分层：Controller → Service → Domain → Mapper
- 异常规范：`.cursor/rules/java-exception-handling.mdc`
- 错误码：BusinessErrorEnum、messages_zh_CN.properties

### 阶段五：MyBatis
- SQL：`**/mapper/**/*.xml` 中的 `<select>`、`<insert>`
- Mapper：接口方法名 = XML 的 id
- 分页：搜索 `PageHelper`

### 阶段六：微服务
- Feign：`*Feign.java`、`*ClientImpl.java`
- Nacos：bootstrap.yml、application.yml

---

## 4. 输出模板示例

讲解某主题时，可按以下结构组织回答：

1. **概念简述**：用 1–2 句话说明是什么
2. **在 scp 中怎么找**：给出具体路径或搜索关键词
3. **简单示例**：引用 scp 中的一段代码（或伪代码）说明
4. **对应 learn 目录**：提醒用户可在 `learn/XX-XXX/YY-主题名/` 下放笔记
