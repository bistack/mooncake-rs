# Mooncake-RS

通过 Rust 重写 Mooncake 分布式 KV 缓存存储系统，深入掌握其核心设计。

## 项目目标

本项目旨在通过 Rust 语言重新实现 Mooncake 的核心组件，从而深入理解其分布式架构、高性能传输引擎和存储设计。通过重写过程，我们可以：

- 掌握 Mooncake 的系统架构和设计理念
- 理解 SGLang 与 Mooncake 的集成机制
- 学习高性能分布式存储系统的设计模式
- 探索 RDMA、零拷贝等先进技术的应用

## 核心设计重点

### 1. SGLang 如何使用 Mooncake

SGLang 是一个高性能语言模型推理框架，使用 Mooncake 作为分布式 KV 缓存后端。集成架构包含：

- **Master Service**：协调和管理整个分布式系统
- **Metadata Service**：可选组件，管理元数据信息
- **Store Service**：可选组件，提供存储服务
- **SGLang Server**：语言模型推理服务器

SGLang 通过 Mooncake 实现：
- **HiCache**：三级缓存架构（GPU内存 L1、主机内存 L2、分布式存储 L3）
- **智能预取**：最小化延迟的预取策略
- **RDMA 加速**：利用 RDMA 技术实现高带宽、低延迟的数据传输
- **零拷贝技术**：减少数据复制，提高传输效率

### 2. Mooncake Transport Engine 设计

传输引擎是 Mooncake 的核心组件，负责高效的数据传输：

- **RDMA 支持**：支持 RoCE 和 InfiniBand 等 RDMA 技术
- **零拷贝传输**：避免不必要的数据复制
- **多路径传输**：支持多种网络路径和传输协议
- **流量控制**：智能的流量管理和拥塞控制
- **错误处理**：健壮的错误检测和恢复机制

关键设计考虑：
- 低延迟：最小化数据传输延迟
- 高吞吐：最大化网络带宽利用率
- 可扩展性：支持大规模分布式部署
- 容错性：网络故障时的优雅降级

### 3. Mooncake Store 设计

存储层负责管理分布式 KV 缓存：

- **分布式哈希表**：高效的数据分片和定位
- **一致性模型**：保证数据一致性的机制
- **缓存策略**：智能的缓存淘汰和更新策略
- **内存管理**：高效的内存分配和回收
- **数据持久化**：可选的数据持久化机制

关键特性：
- **高性能**：支持大规模并发访问
- **低延迟**：快速的数据读写操作
- **可扩展性**：动态添加/移除存储节点
- **容错性**：节点故障时的数据恢复

## Git Submodules

为了便于 AI Code Agent 辅助分析和编程，本项目包含两个 Git 子模块：

### 1. Mooncake

```bash
# 添加 Mooncake 子模块
git submodule add https://github.com/kvcache-ai/Mooncake.git mooncake
```

Mooncake 是原始分布式 KV 缓存存储系统，包含：
- 核心传输引擎实现
- 存储服务实现
- 与 SGLang 的集成代码
- 文档和示例

### 2. SGLang

```bash
# 添加 SGLang 子模块
git submodule add https://github.com/sgl-project/sglang.git sglang
```

SGLang 是高性能语言模型推理框架，包含：
- Mooncake 集成代码
- KV 缓存管理实现
- 推理优化技术
- 示例和测试

## 项目结构

```
mooncake-rs/
├── README.md                 # 本文件
├── mooncake/                 # Mooncake 子模块
│   ├── src/                  # 源代码
│   ├── docs/                 # 文档
│   └── examples/             # 示例
├── sglang/                   # SGLang 子模块
│   ├── python/               # Python 实现
│   ├── docs/                 # 文档
│   └── examples/             # 示例
├── src/                      # Rust 实现
│   ├── transport/            # 传输引擎实现
│   ├── store/                # 存储服务实现
│   ├── master/               # 主控服务实现
│   └── common/               # 公共组件
├── tests/                    # 测试代码
├── docs/                     # 设计文档
└── examples/                 # 使用示例
```

## 开发计划

### 阶段一：分析和理解
- [ ] 阅读 Mooncake 源代码，理解架构设计
- [ ] 分析 SGLang 与 Mooncake 的集成机制
- [ ] 绘制系统架构图和数据流图
- [ ] 识别核心设计模式和算法

### 阶段二：核心组件实现
- [ ] 实现基础传输引擎（支持 TCP/UDP）
- [ ] 实现分布式存储服务
- [ ] 实现主控服务
- [ ] 实现基本的 KV 缓存管理

### 阶段三：高级特性
- [ ] 添加 RDMA 支持
- [ ] 实现零拷贝传输
- [ ] 优化性能和延迟
- [ ] 添加监控和调试工具

### 阶段四：集成测试
- [ ] 与 SGLang 集成测试
- [ ] 性能基准测试
- [ ] 压力测试和稳定性验证
- [ ] 文档完善

## 技术栈

- **语言**：Rust（主要实现）
- **网络**：tokio（异步运行时）、RDMA 相关库
- **存储**：自定义分布式存储引擎
- **序列化**：serde、bincode、protobuf
- **监控**：metrics、tracing
- **测试**：cargo test、集成测试框架

## 如何开始

1. 克隆仓库：
   ```bash
   git clone --recursive https://github.com/yourusername/mooncake-rs.git
   cd mooncake-rs
   ```

2. 初始化子模块：
   ```bash
   git submodule init
   git submodule update
   ```

3. 安装 Rust 工具链：
   ```bash
   curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
   ```

4. 构建项目：
   ```bash
   cargo build
   ```

5. 运行测试：
   ```bash
   cargo test
   ```

## 学习资源

- [Mooncake 官方文档](https://github.com/kvcache-ai/Mooncake)
- [SGLang 官方文档](https://github.com/sgl-project/sglang)
- [RDMA 编程指南](https://www.rdmamojo.com/)
- [Rust 异步编程](https://tokio.rs/)
- [分布式系统设计](https://distributed.systems/)

## 贡献指南

欢迎贡献代码、文档和测试用例！请遵循以下步骤：

1. Fork 项目
2. 创建特性分支
3. 提交更改
4. 推送到分支
5. 创建 Pull Request

## 许可证

本项目采用 MIT 许可证 - 详见 [LICENSE](LICENSE) 文件。

## 致谢

- Mooncake 团队提供的优秀设计和实现
- SGLang 团队的集成工作
- Rust 社区的强大工具链和生态系统

---

**注意**：这是一个学习项目，旨在理解 Mooncake 的核心设计。生产环境请使用原始 Mooncake 项目。