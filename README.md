<h1 align="center">OpenUSim</h1>

<p align="center">
  <strong>ns-3-UB: UnifiedBus Network Simulation Framework</strong><br>
  基于灵衢（UnifiedBus）基础规范构建的 ns-3 仿真模块
</p>

<p align="center">
  <a href="https://gitcode.com/open-usim/ns-3-ub">GitCode</a> ·
  <a href="QUICK_START.md">快速开始</a> ·
  <a href="scratch/README.md">场景指南</a> ·
  <a href="RELEASE_NOTES_UB.md#release-210">发布说明</a> ·
  <a href="README_en.md">English</a>
</p>

<p align="center">
  <a href="https://gitcode.com/open-usim/ns-3-ub"><img src="https://img.shields.io/static/v1?label=GitCode&message=open-usim%2Fns-3-ub&color=C71D23&logo=git&logoColor=white" alt="GitCode repository"></a>
  <a href="https://www.unifiedbus.com"><img src="https://img.shields.io/static/v1?label=spec&message=UnifiedBus%20v2.1&color=111827" alt="UnifiedBus v2.1 specification"></a>
  <a href="RELEASE_NOTES_UB.md#release-210"><img src="https://img.shields.io/static/v1?label=release&message=2.1.0&color=6F42C1" alt="Release 2.1.0"></a>
  <a href="LICENSE"><img src="https://img.shields.io/static/v1?label=license&message=GPL-2.0&color=97CA00" alt="GPL-2.0 license"></a>
  <br>
  <a href="src/unified-bus"><img src="https://img.shields.io/static/v1?label=language&message=C%2B%2B20&color=00599C&logo=cplusplus&logoColor=white" alt="C++20"></a>
  <a href="scratch/ns-3-ub-tools"><img src="https://img.shields.io/static/v1?label=tools&message=Python&color=3776AB&logo=python&logoColor=white" alt="Python tools"></a>
  <a href="https://www.nsnam.org/releases/ns-3-44/"><img src="https://img.shields.io/static/v1?label=ns-3&message=3.44&color=00A0E9" alt="ns-3.44"></a>
  <a href="CMakeLists.txt"><img src="https://img.shields.io/static/v1?label=build&message=CMake%20%2B%20Ninja&color=064F8C&logo=cmake&logoColor=white" alt="CMake and Ninja build"></a>
  <a href="UNISON_README.md"><img src="https://img.shields.io/static/v1?label=parallel&message=Unison%20%2B%20MPI&color=FF8C00" alt="Unison and MPI parallel simulation"></a>
</p>

<p align="center">
  <strong>Release 2.1.0</strong> · 2026 年 9 月<br>
  <br>
  <strong>基于灵衢基础规范 2.1 版本更新仿真模型</strong><br>
  丰富队列事务等事务类型 · 完善传输层模式 · 支持网络层可靠重路由 · 新增包头与包格式 · 数据链路层包率控制<br>
  <br>
  <strong>仿真引擎优化</strong><br>
  精简仿真事件 · 资源按需加载 · 拓扑结构识别 · 快速路由计算 · 降低内存占用<br>
  <br>
  <a href="RELEASE_NOTES_UB.md#release-210">完整发布说明 →</a>
</p>

---

**目录**： [项目概述](#项目概述) · [引用本项目](#引用本项目) · [研究应用](#研究应用) · [项目架构](#项目架构) · [核心组件](#核心组件) · [OpenUSim Skills](#4-openusim-skills仓库内置) · [答疑交流](#答疑交流) · [许可证](#许可证)

## 项目概述

`ns-3-UB` 是基于[灵衢基础规范 2.1 版本](https://www.unifiedbus.com/zh)的 ns-3 仿真模块，实现了功能层、事务层、传输层、网络层和数据链路层的协议框架与配套算法，可用于协议研究、网络架构探索以及拥塞控制、流量控制、负载均衡、路由算法等方向的仿真验证。

本项目基于 ns-3.44 构建。详细的平台支持、安装步骤、系统要求及编译选项，请参阅 [ns-3.44 文档](https://www.nsnam.org/releases/ns-3-44/documentation/)、[安装指南](https://www.nsnam.org/docs/release/3.44/installation/singlehtml/) 及 [ns-3.44 源码](https://gitlab.com/nsnam/ns-3-dev/-/tree/ns-3.44?ref_type=tags)。

本项目已集成 Unison for ns-3 多线程并行仿真能力（[EuroSys '24 paper](https://dl.acm.org/doi/10.1145/3627703.3629574)），更多信息与使用方法参阅 [UNISON_README.md](UNISON_README.md) 和 [QUICK_START.md](QUICK_START.md)。

> 本项目参考灵衢基础规范实现，仿真行为与规范之间可能存在差异。请以灵衢基础规范为权威参考。

`ns-3-UB` 可用于研究：

- 面向流量亲和、低成本、高可靠的拓扑架构。
- 集合通信算子与流量编排算法。
- 总线内存事务成网场景下的事务层保序与可靠性。
- 超节点网络的内存语义传输控制。
- 自适应路由、负载均衡、拥塞控制和 QoS 优化算法。

> 本项目针对规范未指明的策略与算法（如交换机建模方式、路由选择、拥塞标记、缓冲与仲裁策略等）提供可插拔的“参考实现”。这些实现不属于灵衢基础规范的一部分，仅作为示例与基线方案，用户可按需替换或禁用。
>
> 本项目不包含的功能包括但不限于：硬件内部细节建模、物理层、性能参数、控制面行为（如初始化行为、异常事件处理等）、内存管理、安全策略等。

### 典型仿真功能

<table>
  <thead>
    <tr>
      <th align="center">层级</th>
      <th align="center">能力分类</th>
      <th align="left">已支持功能</th>
      <th align="left">未完善 / 用户自定义功能</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td align="center" rowspan="2">功能层</td>
      <td>功能类型</td>
      <td>Load/Store 接口、URMA 接口</td>
      <td>URPC、Entity 相关高级功能</td>
    </tr>
    <tr>
      <td>Jetty</td>
      <td>Jetty、多对多 / 单对单通信模型</td>
      <td>Jetty Group、Jetty 状态机</td>
    </tr>
    <tr>
      <td align="center" rowspan="3">事务层</td>
      <td>事务服务模式</td>
      <td>ROI、ROL、UNO</td>
      <td>ROT</td>
    </tr>
    <tr>
      <td>事务保序</td>
      <td>NO / RO / SO</td>
      <td>—</td>
    </tr>
    <tr>
      <td>事务类型</td>
      <td>Write、Read、Send、Writeback；<a href="src/unified-bus/doc/atomic-support.md">Atomic（网络行为模型）</a>；队列事务</td>
      <td>Write_with_BE、Write_with_remote_sync 等</td>
    </tr>
    <tr>
      <td align="center" rowspan="4">传输层</td>
      <td>传输模式</td>
      <td>RTP、CTP</td>
      <td>UTP</td>
    </tr>
    <tr>
      <td>可靠传输机制</td>
      <td>PSN、超时重传、Go-Back-N、选择性重传、快速选择性重传</td>
      <td>更多用户自定义可靠传输策略</td>
    </tr>
    <tr>
      <td>拥塞控制</td>
      <td>RTP 拥塞控制机制、CAQM、DCQCN</td>
      <td>LDCP、CTP 拥塞控制机制</td>
    </tr>
    <tr>
      <td>多路径负载均衡</td>
      <td>RTP 负载均衡：TPG 机制、逐 TP / 逐包负载均衡、乱序接收</td>
      <td>CTP 负载均衡</td>
    </tr>
    <tr>
      <td align="center" rowspan="5">网络层</td>
      <td>报文格式</td>
      <td>基于 IP / 16-bit CNA / 24-bit CNA 地址格式的包头、TP Bypass（CFG7）</td>
      <td>其余包头格式</td>
    </tr>
    <tr>
      <td>网络地址管理</td>
      <td>CNA 与 IP 地址转换、Primary / Port CNA</td>
      <td>用户可自定义的地址分配与转换策略</td>
    </tr>
    <tr>
      <td>路由查找</td>
      <td>基于目的地址 + 包头 RT 域段的基础路由策略、基于路径 Cost 的路由策略、基于 Hash 的 ECMP、基于负载均衡因子的逐流/逐包 Hash、基于端口负载的自适应路由、可靠重路由</td>
      <td>用户可自定义路由策略</td>
    </tr>
    <tr>
      <td>服务质量</td>
      <td>SL-VL 映射、基于 SP/DWRR 的 VL 间调度</td>
      <td>用户可自定义的 VL 间调度策略</td>
    </tr>
    <tr>
      <td>拥塞标记</td>
      <td>基于包头 CC 域段的 CAQM 标记模式、DCQCN FECN 标记模式</td>
      <td>FECN_RTT 标记模式</td>
    </tr>
    <tr>
      <td align="center" rowspan="5">数据链路层</td>
      <td>数据链路层包头配置</td>
      <td>CFG9（16-bit CNA 地址）、CFG7（24-bit CNA 地址）</td>
      <td>—</td>
    </tr>
    <tr>
      <td>报文收发</td>
      <td>报文粒度建模</td>
      <td>Cell / Flit 粒度建模</td>
    </tr>
    <tr>
      <td>虚拟通道</td>
      <td>点到点链路最多支持 16 个 VL、基于 SP/DWRR 的 VL 间调度</td>
      <td>用户可自定义的 VL 间调度策略</td>
    </tr>
    <tr>
      <td>链路流控</td>
      <td>CBFC（独占信用 / 共享信用）、PFC（固定阈值 / 动态阈值）</td>
      <td>控制面的信用初始化行为、用户自定义信用共享策略</td>
    </tr>
    <tr>
      <td>包率控制</td>
      <td>静态、动态包率控制</td>
      <td>—</td>
    </tr>
  </tbody>
</table>

## 引用本项目

```bibtex
@software{openusim,
  month = {9},
  title = {{OpenUSim: An Open-Source UnifiedBus Network Simulation Framework}},
  url = {https://gitcode.com/open-usim/ns-3-ub},
  version = {2.1.0},
  year = {2026}
}
```

<!-- BEGIN PUBLICATIONS -->
### 研究应用

以下优秀工作使用 OpenUSim 进行仿真验证：

1. BAO J, ZHU W, ZHAO G, et al. OpDisPath: Optimal Communication Scheduling in GPU Clusters Through Disjoint Path Selection[J]. IEEE Transactions on Networking, 2026, 34: 8011-8026. DOI: [10.1109/ton.2026.3748714](https://doi.org/10.1109/ton.2026.3748714).

2. LIU S, KONG D, WANG H, et al. Balanced Sparse Tree: A Scalable Network Topology for Large Language Models[C]//Proceedings of the ACM SIGCOMM 2026 Conference. New York, NY, USA: ACM, 2026: 676-695. DOI: [10.1145/3789240.3829208](https://doi.org/10.1145/3789240.3829208).

3. WANG Y, ZHANG J, HUANG X, et al. Argus: Scalable and Deterministic Network Fault Localization for AI Training Clusters[C]//Proceedings of the 10th Asia-Pacific Workshop on Networking. New York, NY, USA: ACM, 2026: 51-58. DOI: [10.1145/3820441.3820449](https://doi.org/10.1145/3820441.3820449).

4. LONG F, GAO K, CHEN L, et al. Supercharging Packet-level Network Simulation of Large Model Training via Memoization and Fast-Forwarding[C]//23rd USENIX Symposium on Networked Systems Design and Implementation (NSDI 26). Renton, WA: USENIX Association, 2026: 1131-1151. [Paper](https://www.usenix.org/conference/nsdi26/presentation/long).
<!-- END PUBLICATIONS -->

## 项目架构

```
├── README.md                    # 项目说明文档
├── scratch/                     # 仿真示例和测试用例
│   ├── ub-quick-example.cc      # 主要仿真程序
│   ├── 2nodes*/                 # 简单双节点拓扑测试用例
│   ├── clos*/                   # CLOS 拓扑测试用例
│   └── 2dfm4x4*/                # 2D FullMesh 4x4 测试用例
│
└── src/unified-bus/             # 基于灵衢基础规范的仿真组件
    ├── model/
    │   ├── protocol/
    │   │   └── ub-*             # 协议栈相关建模组件
    │   └── ub-*                 # 网元和算法组件
    ├── test/                    # 单元测试
    └── doc/                     # 文档和图表
```

## 核心组件

### 1. UnifiedBus (UB) 模块

UB 模块是基于灵衢基础规范实现的仿真组件：

#### 网元建模组件

<p align="center">
<img src="src/unified-bus/doc/figures/arch2-light.D3-LpLKH.png" alt="UB Domain 系统组成" width="85%">
<br>
<em>UB Domain 系统组成架构图。来源：www.unifiedbus.com</em>
</p>

- **UB Controller** (`ub-controller.*`) - 执行 UB 协议栈的关键组件，同时为用户提供接口
- **UB Switch** (`ub-switch.*`) - 用于 UB 端口间数据转发
- **UB Port** (`ub-port.*`) - 端口抽象，处理数据包输入输出
- **UB Link** (`ub-link.*`) - 节点间的点到点连接

#### 协议栈组件

- **编程接口实例** (`ub-ldst-instance*`, `ub-ldst-thread*`, `ub-ldst-api*`) - Load/Store 编程接口实例，用于对接功能层编程模型
- **UB Function** (`ub-function.*`) - 功能层协议框架实现，支持 Load/Store 与 URMA 编程模型
- **UB Transaction** (`ub-transaction.*`) - 事务层协议框架实现
- **UB Transport** (`ub-transport.*`) - 传输层协议框架实现
- **UB Network** (由 `ub-routing-table.*`、`ub-congestion-control.*`、`ub-switch.*` 组成) - 网络层协议框架实现
- **UB Datalink** (`ub-datalink.*`) - 数据链路层协议框架实现

#### 网络算法组件

- **流量注入组件** (`ub-traffic-gen.*`) - 读取用户流量配置，为仿真节点按串并行关系注入流量
- **TP Connection Manager** (`ub-tp-connection-manager.h`) - TP Channel 管理器，方便用户查找各节点 TP Channel 信息
- **Switch Allocator** (`ub-switch-allocator.*`) - 建模交换机的报文调度与出端口仲裁
- **Queue Manager** (`ub-queue-manager.*`) - 缓冲区管理模块，影响负载均衡、流量控制、排队、丢包等行为
- **Routing Process** (`ub-routing-process.*`) - 路由表模块，实现了路由表的管理与查询功能
- **Congestion Control** (`ub-congestion-control.*`) - 拥塞控制算法框架模块
- **拥塞控制算法** (`ub-caqm.*`, `ub-dcqcn.*`) - CAQM 与 DCQCN 拥塞控制算法实现
- **Flow Control** (`ub-flow-control.*`) - 流量控制框架模块
- **故障注入模块** (`ub-fault.*`) - 用于在特定流量过程中注入丢包率、高时延、拥塞程度、错包、闪断、降 lane 等故障参数

#### 数据类型和工具

- **Datatype** (`ub-datatype.*`) - UB 数据类型定义
- **Header** (`ub-header.*`) - UB 协议包头定义和解析
- **Network Address** (`ub-network-address.h`) - 网络地址相关工具函数，包含地址转换、掩码匹配等功能

### 2. 核心仿真特性

#### 拓扑和流量支持

- **任意拓扑**：支持任意拓扑的仿真与建模，用户可基于 UB 工具集快速构建拓扑与路由表
- **任意流量**：支持任意仿真流量配置，可通过 UB 工具集构建集合通信与通信算子图
- **性能监控**：收集和分析仿真性能指标

#### 协议栈建模

- **UB 协议栈**：支持数据链路层、网络层、传输层、事务层和功能层建模
- **内存语义**：实现基于 Load/Store 的内存语义行为建模
- **消息语义**：实现基于 URMA 的消息语义行为建模
- **原生多路径**：通过 TP/TP Group 协议机制实现原生多路径支持

#### 协议算法支持

- **流量控制**：实现基于信用的流量控制（CBFC 独占/共享信用）和基于优先级的流量控制（PFC 固定阈值/动态阈值）
- **拥塞控制**：实现拥塞控制算法常用的网侧标记、接收端回复、发送端响应框架，支持 CAQM 与 DCQCN 算法
- **路由策略**：支持最短路由与绕路，提供 Hash、轮询、自适应和入端口条带等路径选择策略
- **QoS 支持**：提供端到端 QoS 支持，当前支持 SP 与 DWRR 调度策略
- **交换仲裁**：模块化实现 UB Switch 的交换仲裁机制建模，当前支持 SP 与 DWRR 调度

路由策略的选择与配置说明见[路由策略指南](.agents/skills/openusim-references/routing-strategy-selection.md)。

### 3. 脚本工具集

脚本工具集支持以下仿真流程：

- **网络拓扑生成**：自动生成各种网络拓扑（Clos、2D FullMesh 等）
- **流量模式生成**：支持 All-Reduce、All-to-All、All-to-All-V 等通信模式，支持 RHD、NHR、OneShot 多种集合通信算法
- **性能分析工具**：吞吐量计算、延迟分析、CDF 绘制
- **格式化结果输出**：自动生成流完成时间、带宽等基础结果信息表格，可选生成报文粒度网内逐跳信息。

### 4. OpenUSim Skills（仓库内置）

本仓库在 `.agents/skills/` 目录下维护一组 OpenUSim Skills，用于配合 Agent 规划、运行和分析 `ns-3-ub` 仿真。

这些 skills 支持单次仿真、A/B 对比、参数扫描和控制变量实验，使用说明见 [OpenUSim Skills 文档](.agents/skills/README.md)。

主要流程包括四个阶段：

- `openusim-welcome`：检查当前工作树、构建产物和工具链是否可用
- `openusim-plan-experiment`：把自然语言目标整理成可执行的仿真方案
- `openusim-run-experiment`：生成 case、补齐配置、执行仿真，并记录运行错误
- `openusim-analyze-results`：解释输出、分析异常，并给出下一轮实验建议

可选的 `openusim-capture-insights` 用于在用户同意后，把已经确认的根因或通用结论写成知识卡。

上述 Skills 与主仓代码及 `ns-3-ub-tools` 子模块紧密耦合，随仓库统一维护。

## 答疑交流

欢迎加入 OpenUSim 仿真答疑交流微信群：

<p>
<img src="https://raw.gitcode.com/user-images/assets/7654616/5e10e044-01db-4f9c-8614-dbb9e2bd4804/image.png" width="300" height="300" alt="OpenUSim 答疑交流微信群二维码">
</p>

## 许可证

本项目遵循 ns-3 许可证协议，GPL v2。详见 [`LICENSE`](LICENSE) 文件。

---

<p align="center">
  <sub>基于 <a href="https://www.unifiedbus.com/zh">灵衢基础规范</a> 构建 · 基于 <a href="https://www.nsnam.org/releases/ns-3-44/">ns-3.44</a> · GPL-2.0 License</sub>
</p>
