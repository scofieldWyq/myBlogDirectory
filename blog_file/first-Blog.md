title: first_Blog
date: 2016-02-21 11:12:02
tags:
---

# Core Data For IOS - 笔记
> **如果不能把一件事用简单的话说清楚， 那就表明你理解得还不够透彻。----- 阿尔伯特＊爱因斯坦**

## Core Data:

- Core Data 是一个框架， 是的开发者可以把数据当成对象来操作，而不必在乎数据在磁盘中的存储方式。

## 概念:

- 托管对象(managed object): NSManagedObject 类或者子类。
  - Core Data 提供的数据对象
- 持久化存储区(persistent store)
  - 磁盘内容区域
- 托管对象模型
  - 把数据从托管对象映射到持久化存储区的作用 
- 托管对象上下文(managed object context)
  - 所有托管对象存放的地方
  - 托管对象上下文位于高速RAM中，有了托管对象上下文之后，对于原来需要读取磁盘才能获取的数据，现在只需访问这个上下文久可以了
  - 内存和磁盘的桥梁
  
> 托管对象持有一份对持久化存储区里相关数据的拷贝， 如果数据库作为持久化存储区，那么托管对象就是可能对应于数据库里某张数据表的一行。

### 持久化存储协调器(Persistent store coordinator)

- 保存一份持久化存储区
- 选用SQLite数据库作为持久化存储区
- 可以有多个持久化存储区
- 创建持久化存储区的类:`NSPersistentStore`
- 创建持久化存储协调器的类:`NSPersistentStoreCoordinator`

### 托管对象模型(managed object model)

- NSManagedObjectModel
- 数据结构的描述或者图示
- 类似于类，而不是对象实例
- 托管对象是托管对象模型的实例

### 托管对象上下文

- 包含多个托管对象

### 总结

- `NSManagedObject` (托管对象)存放在 `NSManagedObjectContext` (托管对象上下文)
- `NSManagedObjectModel` (托管对象模型) 作为托管对象实例的模版
- `NSPersistentStore` (持久化存储区) 存放在 `NSPersistentStoreCoordinator` (持久化存储协调器) 中
- `NSManagedObjectContext` 和 `NSPersistentStoreCoordinator` 有联系
- 所有对象模型绑定在持久化存储协调器
- 可以通过模型的标示去获得实例或者直接在托管对象上下文操作数据
