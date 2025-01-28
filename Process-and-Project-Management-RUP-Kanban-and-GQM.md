---
title: 'Process and Project Management: RUP, Kanban and GQM'
date: 2025-01-27 19:59:45
categories: 学习笔记
tags: 
- Process and Project Management
- Software Engineering
- Development Method
- CMU
---

This Note contains: 

- RUP
- GQM
- Kanban

<!-- more -->
<!-- toc -->

### **1. What is the Rational Unified Process (RUP)?**

- A **software engineering process** that assigns tasks and responsibilities within a development organization.
- Aims to produce **high-quality software** that meets end-user needs within a **predictable schedule and budget**.
- **Enhances team productivity** by providing access to guidelines, templates, and tool mentors.
- **Emphasizes models over documents**, promoting **Unified Modeling Language (UML)**.
- **Configurable process**, adaptable for small and large teams.

### **2. Six Best Practices of RUP**

1. **Develop Software Iteratively**
    - Reduces risk by allowing continuous feedback.
    - Each iteration ends with an **executable release**.
    - Helps accommodate changes in requirements.
2. **Manage Requirements**
    - Uses **use cases** and **scenarios** to define and track requirements.
    - Ensures alignment between requirements, design, implementation, and testing.
3. **Use Component-Based Architectures**
    - Focuses on **early development of a robust architecture**.
    - Encourages **software reuse** and **modular development**.
4. **Visually Model Software**
    - Uses UML for **graphical representation** of software structure and behavior.
    - Ensures clarity, consistency, and effective communication among team members.
5. **Verify Software Quality**
    - Incorporates **continuous testing** throughout development.
    - Quality is assessed based on reliability, functionality, and performance.
6. **Control Changes to Software**
    - Provides a structured process for **tracking and managing changes**.
    - Establishes **secure workspaces** and **automated integration**.

### **3. RUP Process Overview**

- Two Dimensions:
    - **Time Dimension** (Phases & Iterations)
    - **Static Dimension** (Activities, Artifacts, Workers, Workflows)

#### **Phases of RUP (Time Dimension)**

1. **Inception Phase**: Define business case, project scope, and risks.
2. **Elaboration Phase**: Establish architecture, analyze risks, and refine requirements.
3. **Construction Phase**: Develop and integrate components; emphasize efficiency.
4. **Transition Phase**: Deliver software to users, conduct training, and fix issues.

#### **Iterations**

- Each phase consists of **multiple iterations**, allowing incremental improvements.
- Benefits of iterative development:
    - **Reduces risk**
    - **Handles changes better**
    - **Encourages software reuse**
    - **Improves overall quality**

### **4. Core Workflows (Static Dimension)**

- **Six Core Engineering Workflows:**
    1. **Business Modeling**: Ensures software aligns with business needs.
    2. **Requirements**: Captures use cases and defines system behavior.
    3. **Analysis & Design**: Develops architecture and design models.
    4. **Implementation**: Organizes code into **implementation subsystems**.
    5. **Testing**: Validates system reliability and functionality.
    6. **Deployment**: Manages product release and user adoption.
- **Three Supporting Workflows:**
    1. **Project Management**: Plans and monitors software development.
    2. **Configuration & Change Management**: Controls artifact versions and changes.
    3. **Environment**: Provides the tools and processes for development.

### **5. Tools Supporting RUP**

- **Rational Requisite Pro**: Requirement tracking.
- **Rational Rose**: Visual modeling tool (UML).
- **Rational Clear Quest**: Change request management.
- **Rational Team Test**: Automated functional testing.
- **Rational ClearCase**: Configuration management.
- **Rational SoDA**: Automated documentation generation.

### **6. History of RUP**

- **Evolved from Objectory Process (by Ivar Jacobson, 1987)**.
- **Merged with Rational Software and adopted UML**.
- **Incorporated iterative development, architecture focus, and project management**.

### **7. Introduction to the Goal Question Metric (GQM) Approach**

- **Purpose of Measurement in Software Development**:
    - Helps in **project planning** (e.g., cost estimation).
    - Assesses **processes and products** (e.g., error frequency, defect density).
    - Supports **decision-making** on software quality improvement.
    - Allows tracking progress and taking **corrective actions**.
- **Effective Measurement Must Be**:
    1. **Goal-focused** (i.e., must be tied to business or project objectives).
    2. **Applied across all life-cycle products, processes, and resources**.
    3. **Interpreted in the context of organizational goals**.

### **8. What is the Goal Question Metric (GQM) Approach?**

- **Main Idea**:

    - Organizations should define **goals**, link them to specific **questions**, and then derive **metrics** to measure progress.

- **Origins**:

    - Originally developed at NASA's **Goddard Space Flight Center**.
    - Evolved to be part of the **Quality Improvement Paradigm** and **Experience Factory model**.

- **Key Output of GQM**:

    - A 

        measurement system

         with:

        1. Defined **goals**.
        2. **Questions** that break down goals into measurable aspects.
        3. **Metrics** that provide quantifiable answers to those questions.

### **9. Three Levels of GQM Model**

1. **Conceptual Level (GOAL)**:
    - Defines objectives for:
        - **Products** (e.g., software specifications, designs).
        - **Processes** (e.g., testing, development activities).
        - **Resources** (e.g., team members, tools, infrastructure).
    - Goals are defined with respect to **quality models** and specific viewpoints.
2. **Operational Level (QUESTION)**:
    - Questions help assess **goal achievement**.
    - They describe how to evaluate **product/process quality**.
    - Example: *How many defects are found per testing cycle?*
3. **Quantitative Level (METRIC)**:
    - **Objective Metrics**: Independent of perspective (e.g., number of defects, lines of code, staff hours).
    - **Subjective Metrics**: Depend on perspective (e.g., user satisfaction, readability of documentation).

### **10. Structure of a GQM Model**

- **A hierarchical breakdown**:
    - **Goals → Questions → Metrics**.
    - Goals are refined into **several questions**.
    - Each question is **answered using metrics**.
    - The same metric can be used for different questions.
- **Example of a GQM Model for Change Request Processing**:
    - **Goal**: Improve the timeliness of processing change requests (from the project manager’s viewpoint).
    - Questions:
        1. What is the current change request processing speed?
        2. Is the performance improving?
        3. Are the required steps actually being followed?
    - Metrics:
        - Average cycle time.
        - Standard deviation in processing time.
        - Number of cases outside acceptable limits.
        - Subjective ratings by project managers.

### **11. The GQM Process: How It Works**

1. **Identify Goals**:
    - Goals come from **corporate strategies, policies, and business needs**.
    - Goals should align with **quality improvement objectives**.
2. **Define Questions**:
    - Questions should help **evaluate** whether the goal is being met.
    - At least three types:
        - **Characterization questions** (e.g., How does the process work currently?).
        - **Performance evaluation questions** (e.g., Is the process getting better?).
        - **Satisfaction questions** (e.g., Is management satisfied with the improvements?).
3. **Define Metrics**:
    - Use a combination of **existing data** and **newly collected data**.
    - Consider **objectivity** (e.g., numerical data) vs. **subjectivity** (e.g., opinions, surveys).
    - Ensure metrics support **continuous improvement and learning**.
4. **Data Collection & Analysis**:
    - Choose appropriate **data collection methods**.
    - Ensure **reliable interpretation** using predefined rules.

### **12. Applications of GQM**

- **Used in software quality and process improvement programs**.
- Applied in companies like **NASA, Hewlett-Packard, Motorola, and Coopers & Lybrand**.
- Can be integrated into broader **software engineering strategies** (e.g., Experience Factory model).

### **13. Introduction to Kanban for Agile Teams**

- **Kanban is a lightweight framework** for managing work, often integrated with Agile methodologies like Scrum.

- Three primary steps

    in implementing Kanban:

    1. **Visualize the process** (using a Kanban board).
    2. **Limit Work in Process (WIP)** (to improve efficiency).
    3. **Manage Lead Time** (optimize flow and remove bottlenecks).

### **14. The Kanban Board**

- A **visual tool** that represents the **flow of work** through a process.
- Resembles a Scrum task board but has **key differences**:
    - Focuses on **continuous flow** rather than iterations.
    - Uses **WIP limits** to control work quantity in each column.
    - Allows for **pull-based** task management (work moves forward when capacity allows).
- **Common Kanban Board Columns**:
    - **Next** (Ready for work)
    - **Analysis**
    - **Development**
    - **Acceptance**
    - **Production (Done)**

### **15. Visual Control System**

- Kanban boards act as a **Visual Control System** that enhances transparency.
- Examples:
    - **Tool rack silhouettes** show where tools belong.
    - **Assignment boards** track team members' work using color-coded magnets.
- Benefits:
    - **Improves communication** across the team.
    - **Identifies bottlenecks** immediately.
    - **Encourages self-organization** without direct managerial intervention.

### **16. Work in Process (WIP) Limits**

- **Definition**: WIP limits **restrict** the number of work items allowed in each column.

- **Why is WIP Limiting Important?**

    - Reduces **Lead Time** (total time from start to completion).
    - Prevents **task switching**, which increases efficiency.
    - Encourages **teamwork** by focusing on clearing bottlenecks.

- **Little’s Law** explains the relationship:
    $$
    {\text{Lead Time} = \frac{\text{WIP}}{\text{Cycle Time}}}
    $$


- To **reduce Lead Time**, lower the WIP limit.

- **Scrum vs. Kanban on WIP**:

    - Scrum **implicitly** controls WIP through sprint planning.
    - Kanban **explicitly** limits WIP at each stage of work.

### **17. Constraints in Kanban**

- **WIP limits expose system constraints** by highlighting stages where work piles up.
- When a constraint is identified:
    - The team **stops pulling new work** until the bottleneck is resolved.
    - Encourages **collaborative problem-solving**.
- Difference from Scrum:
    - Scrum addresses issues **retrospectively** (after a sprint).
    - Kanban **actively reacts** to constraints in **real-time**.

### **18. Flow Over Utilization**

- Traditional organizations focus on **100% resource utilization**.
- Kanban **prioritizes flow** over full utilization.
- Key concepts:
    - **Flow efficiency** is more important than keeping people constantly busy.
    - Specializing generalists (team members with overlapping skills) improve adaptability.

### **19. Planning for Variability**

- Software development is **inherently variable** (uncertain task durations, changing requirements).
- Kanban **embraces variability** instead of trying to eliminate it.
- Approach:
    - Use **small batch sizes** to accommodate changes.
    - Adjust WIP limits based on observed throughput.

### **20. No Estimates or Iterations?**

- **Kanban doesn’t require fixed iterations** (like Scrum’s sprints).
- **Does not rely on estimates**:
    - Focuses on **tracking Lead Time** instead.
    - Uses **historical data** to predict delivery times.
- **Comparison of Metrics**:
    - **Scrum**: Velocity (story points per sprint).
    - **Kanban**: Lead Time (average time to complete a task).

### **21. When to Use Kanban?**

- Best suited for teams that:
    - Have **highly variable workloads**.
    - Need **continuous delivery** rather than fixed iterations.
    - Want to **optimize efficiency** by limiting WIP.
- Can be used alongside **Scrum (Scrumban)** to balance predictability and flexibility.

### **22. Kanban in the Enterprise**

- Kanban is **scalable** to large organizations.
- Benefits in enterprise settings:
    - Improves **predictability** and **due date performance**.
    - Helps **manage dependencies** between teams.
    - Enhances **collaboration** across different business units.


## 中文版


### **1. 什么是 Rational Unified Process（RUP）？**

- **软件工程过程（Software Engineering Process）**：RUP 是一种**软件工程过程**，用于在开发组织中**分配任务和责任**，确保高效的软件开发。
- 目标（Aims）：
    - 目标是开发出**高质量软件**（high-quality software）。
    - 该软件要满足**最终用户的需求**（meets end-user needs）。
    - 要求在**可预测的时间表和预算**（predictable schedule and budget）内完成开发。
- 提高团队生产力（Enhances team productivity）：
    - 通过提供**指南（guidelines）**、**模板（templates）** 和**工具指导（tool mentors）**来提高开发效率。
- 模型优先（Emphasizes models over documents）：
    - 强调使用**模型化方法**，而非传统的文档驱动开发。
    - 推广使用**统一建模语言（UML，Unified Modeling Language）** 来可视化系统设计和架构。
- 可配置性（Configurable process）：
    - RUP 是一种**可配置的开发过程**（configurable process）。
    - 适用于**小型团队（small teams）** 和**大型企业（large teams）**，可以根据不同的项目需求进行调整。

### **2. RUP 的六大最佳实践（Six Best Practices of RUP）**

#### **1. 迭代式开发（Develop Software Iteratively）**

- 降低风险（Reduces risk）：
    - 采用迭代开发可以**持续获取用户反馈**，并在早期识别和解决问题，从而降低项目风险。
- 每次迭代结束都会产生一个可执行版本（Each iteration ends with an executable release）：
    - 迭代结束后，团队会交付一个**可运行的版本**，确保每个阶段都有实际产出。
- 适应需求变更（Helps accommodate changes in requirements）：
    - 迭代开发支持**需求变更**，可以根据用户反馈或市场需求随时调整功能。

#### **2. 需求管理（Manage Requirements）**

- 使用用例和场景（Uses use cases and scenarios）：
    - 用**用例（Use Cases）** 和**场景（Scenarios）** 来定义和跟踪软件需求。
- 确保需求、设计、实现和测试保持一致（Ensures alignment between requirements, design, implementation, and testing）：
    - 需求管理贯穿整个开发过程，确保需求、设计、实现和测试相互匹配，避免出现偏差。

#### **3. 组件化架构（Use Component-Based Architectures）**

- 关注早期架构开发（Focuses on early development of a robust architecture）：
    - 在早期就设计并稳定**软件架构**，为后续开发提供坚实基础。
- 鼓励软件复用（Encourages software reuse）：
    - 采用**组件化（Component-Based）** 方式，使不同项目可以**复用已有的组件**，降低开发成本，提高效率。
- 支持模块化开发（Encourages modular development）：
    - 软件被拆分成多个**独立的模块**，提升可维护性和可扩展性。

#### **4. 可视化建模（Visually Model Software）**

- 采用 UML 进行建模（Uses UML for graphical representation）：
    - 使用**UML（Unified Modeling Language，统一建模语言** 来创建软件的 **结构和行为图**。
- 确保清晰度、一致性和有效沟通（Ensures clarity, consistency, and effective communication）：
    - UML **可视化** 软件设计，**减少沟通误解**，确保开发人员、架构师和测试人员的**一致理解**。

#### **5. 软件质量验证（Verify Software Quality）**

- 在整个开发过程中进行持续测试（Incorporates continuous testing throughout development）：
    - 质量控制不是等软件完成后才开始，而是贯穿整个开发生命周期。
- 质量评估标准（Quality is assessed based on reliability, functionality, and performance）：
    - 质量检查包括：
        - **可靠性（Reliability）**：软件能否长期稳定运行。
        - **功能性（Functionality）**：软件是否符合需求规格。
        - **性能（Performance）**：软件是否高效运行。

#### **6. 控制软件变更（Control Changes to Software）**

- 提供结构化流程（Provides a structured process for tracking and managing changes）：
    - RUP 提供了一套**标准化的变更管理流程**，帮助团队有效管理变更，减少版本冲突。
- 建立安全工作环境（Establishes secure workspaces and automated integration）：
    - 采用**版本控制（Version Control）**和**自动化集成（Automated Integration）**技术，保证变更不会影响系统稳定性。

### **3. RUP 过程概述（RUP Process Overview）**

RUP 由**两个维度（Two Dimensions）** 组成：

1. **时间维度（Time Dimension）**：包括**阶段（Phases）** 和 **迭代（Iterations）**。
2. **静态维度（Static Dimension）**：包括**活动（Activities）、工件（Artifacts）、角色（Workers）、工作流（Workflows）**。

#### **RUP 的四个阶段（Phases of RUP - Time Dimension）**

1. **初始阶段（Inception Phase）**：
    - 确定**商业目标（Business Case）**、**项目范围（Project Scope）** 和**关键风险（Risks）**。
2. **细化阶段（Elaboration Phase）**：
    - 确立**架构（Architecture）**，分析**风险（Risks）**，细化**需求（Requirements）**。
3. **构建阶段（Construction Phase）**：
    - 进行**开发和集成（Develop and Integrate Components）**，强调开发**高效性（Efficiency）**。
4. **交付阶段（Transition Phase）**：
    - **交付软件给最终用户（Deliver software to users）**，提供**用户培训（Training）**，修复**遗留问题（Fix Issues）**。

#### **RUP 的迭代（Iterations）**

- 每个阶段包含**多个迭代（Multiple Iterations）**，逐步改进产品。
- 迭代开发的主要优势：
    - **降低风险（Reduces Risk）**。
    - **更好地处理变更（Handles Changes Better）**。
    - **促进软件复用（Encourages Software Reuse）**。
    - **提高整体质量（Improves Overall Quality）**。


### **4. 核心工作流（Core Workflows - Static Dimension）**

#### **六大核心工程工作流（Six Core Engineering Workflows）**

1. **业务建模（Business Modeling）**：确保软件与**业务需求一致**。
2. **需求（Requirements）**：使用**用例** 记录系统功能需求。
3. **分析与设计（Analysis & Design）**：创建**架构和设计模型**。
4. **实现（Implementation）**：管理代码和**开发子系统**。
5. **测试（Testing）**：**验证系统可靠性和功能**。
6. **部署（Deployment）**：管理**软件发布**和**用户使用**。

#### **三大支持性工作流（Three Supporting Workflows）**

1. **项目管理（Project Management）**：**计划和监控开发过程**。
2. **配置与变更管理（Configuration & Change Management）**：**控制版本和变更**。
3. **环境（Environment）**：提供开发工具和基础设施支持。

### **5. RUP 相关工具（Tools Supporting RUP）**

- **Rational Requisite Pro**：需求管理工具。
- **Rational Rose**：UML 建模工具。
- **Rational ClearQuest**：变更请求管理。
- **Rational TeamTest**：自动化测试工具。
- **Rational ClearCase**：版本控制管理。
- **Rational SoDA**：文档自动生成工具。

### **6. RUP 的历史（History of RUP）**

- **1987 年** Ivar Jacobson 提出了 **Objectory 过程**。
- 后来**与 Rational Software 合并**，并采用**UML**。
- 进一步**引入迭代开发、架构设计和项目管理**。

### **7. GQM 方法简介 (Introduction to the Goal Question Metric (GQM) Approach)**

- **软件开发中的度量目的 (Purpose of Measurement in Software Development)**:
    - 帮助进行**项目规划** (例如, 进行成本估算)
    - 评估**软件过程和产品** (例如, 错误频率, 缺陷密度)
    - 支持**质量改进的决策**
    - 允许**跟踪进度**, 并采取**纠正措施**
- **有效的度量方法必须具备以下特点 (Effective Measurement Must Be)**:
    1. **目标驱动 (Goal-focused)** (即, 必须与业务目标或项目目标保持一致)
    2. **适用于整个生命周期 (Applied across all life-cycle products, processes, and resources)**
    3. **能够在组织目标的上下文中进行解释 (Interpreted in the context of organizational goals)**

### **8. 什么是 GQM 方法 (What is the Goal Question Metric (GQM) Approach)?**

- **核心思想 (Main Idea)**:

    - 组织应先定义**目标 (Goals)**, 然后将目标映射到**具体问题 (Questions)**, 最后使用**度量 (Metrics)** 来衡量目标的实现情况

- **起源 (Origins)**:

    - 最初由 NASA **戈达德航天中心 (Goddard Space Flight Center)** 开发
    - 之后被扩展为**质量改进范式 (Quality Improvement Paradigm, QIP)** 和**经验工厂模型 (Experience Factory Model)** 的一部分

- **GQM 的关键成果 (Key Output of GQM)**:

    - 一个度量系统 (A measurement system)

        ，包括:

        1. **明确的目标 (Defined Goals)**
        2. **能够将目标细化为可衡量方面的问题 (Questions that break down goals into measurable aspects)**
        3. **提供可量化答案的度量指标 (Metrics that provide quantifiable answers to those questions)**

### **9. GQM 模型的三个层次 (Three Levels of GQM Model)**

1. **概念层 (Conceptual Level - GOAL)**:
    - 目标定义 (Defines objectives for):
        - **产品 (Products)** (例如, 软件规范, 设计文档)
        - **过程 (Processes)** (例如, 测试, 开发活动)
        - **资源 (Resources)** (例如, 团队成员, 工具, 基础设施)
    - 目标需要结合**质量模型 (Quality Models)** 和**特定的视角 (Viewpoints)** 进行定义
2. **操作层 (Operational Level - QUESTION)**:
    - 通过问题来**评估目标的实现情况**
    - 问题用于分析**产品、过程或资源**的质量
    - 例如: *每个测试周期发现多少个缺陷? (How many defects are found per testing cycle?)*
3. **量化层 (Quantitative Level - METRIC)**:
    - **客观度量 (Objective Metrics)**: 独立于主观因素 (例如, 缺陷数, 代码行数, 工时)
    - **主观度量 (Subjective Metrics)**: 受个人视角影响 (例如, 用户满意度, 代码可读性)

### **10. GQM 模型的结构 (Structure of a GQM Model)**

- **GQM 采用层级结构 (A hierarchical breakdown)**:

    - **目标 → 问题 → 度量 (Goals → Questions → Metrics)**
    - 目标会被细化成多个**关键问题**
    - 每个问题都会通过**度量指标**进行回答
    - **同一个度量可以用于多个问题**

- **示例: 变更请求处理的 GQM 模型 (Example of a GQM Model for Change Request Processing)**:

    - **目标 (Goal)**: 提高变更请求处理的及时性 (Improve the timeliness of processing change requests)

    - 问题 (Questions):

        1. 当前变更请求的处理速度是多少? (What is the current change request processing speed?)
        2. 变更处理流程的性能是否在提高? (Is the performance improving?)
        3. 变更处理是否按照预期步骤执行? (Are the required steps actually being followed?)

    - 度量 (Metrics):

        - 平均处理周期 (Average cycle time)
        - 处理时间的标准差 (Standard deviation in processing time)
        - 超出可接受范围的请求数量 (Number of cases outside acceptable limits)
        - 项目经理的主观评分 (Subjective ratings by project managers)

### **11. GQM 过程 (The GQM Process: How It Works)**

1. **确定目标 (Identify Goals)**:
    - 目标通常来源于**企业战略、政策或业务需求 (corporate strategies, policies, and business needs)**
    - 目标应与**质量改进目标 (quality improvement objectives)** 保持一致
2. **定义问题 (Define Questions)**:
    - 关键问题用于评估目标是否达成
    - 至少包括以下三类问题:
        - **特性描述问题 (Characterization questions)** (例如: 这个过程是如何运作的?)
        - **性能评估问题 (Performance evaluation questions)** (例如: 这个过程是否有所改进?)
        - **满意度评估问题 (Satisfaction questions)** (例如: 管理层是否对改进感到满意?)
3. **定义度量 (Define Metrics)**:
    - 结合**已有数据 (existing data)** 和**新收集的数据 (newly collected data)**
    - 选择**客观指标 (objective metrics)** 和**主观指标 (subjective metrics)** 进行综合评估
    - 确保度量支持**持续改进和学习 (continuous improvement and learning)**
4. **数据收集与分析 (Data Collection & Analysis)**:
    - 选择**合适的数据收集方法 (Choose appropriate data collection methods)**
    - 采用**预定义的规则 (predefined rules)** 确保数据的可靠性

### **12. GQM 的应用 (Applications of GQM)**

- **用于软件质量和过程改进 (Used in software quality and process improvement programs)**

- 已成功应用于以下企业:

    - **NASA (美国国家航空航天局)**
    - **Hewlett-Packard (惠普)**
    - **Motorola (摩托罗拉)**
    - **Coopers & Lybrand (库珀斯&莱布兰)**

- **可以集成到更广泛的软件工程策略中 (Can be integrated into broader software engineering strategies)**，例如**经验工厂模型 (Experience Factory model)**


### **13. 敏捷团队中的 Kanban 介绍 (Introduction to Kanban for Agile Teams)**

- **Kanban 是一种轻量级框架 (Kanban is a lightweight framework)**，用于管理工作流程，通常与**Scrum 等敏捷方法结合使用**。
- **实施 Kanban 的三大核心步骤 (Three primary steps in implementing Kanban)**：
    1. **可视化流程 (Visualize the process)**：使用**看板 (Kanban Board)** 展示任务流转情况。
    2. **限制在制品 (Limit Work in Process, WIP)**：控制每个阶段的工作数量，提高效率。
    3. **管理交付时间 (Manage Lead Time)**：优化流动性，消除瓶颈。

### **14. Kanban 看板 (The Kanban Board)**

- **Kanban 看板是一种可视化工具 (A visual tool that represents the flow of work through a process)**，用于跟踪工作进度。
- **与 Scrum 任务板的主要区别 (Key differences from a Scrum task board)**：
    - **专注于持续流动 (Continuous flow)**，而非固定迭代 (Iterations)。
    - **通过 WIP 限制 (Work In Process Limits) 控制任务数量**，确保流程稳定。
    - **采用拉动式管理 (Pull-based task management)**，任务根据可用资源向前流动。
- **典型的 Kanban 看板列 (Common Kanban Board Columns)**：
    - **Next** (准备开始)
    - **Analysis** (分析阶段)
    - **Development** (开发阶段)
    - **Acceptance** (验收阶段)
    - **Production (Done)** (生产完成)

### **15. 可视化控制系统 (Visual Control System)**

- **Kanban 看板充当可视化控制系统 (Kanban boards act as a Visual Control System)**，提供**工作透明度**。
- 示例 (Examples)：
    - **工具架轮廓 (Tool rack silhouettes)** 指示工具存放位置。
    - **任务分配板 (Assignment boards)** 通过**彩色磁铁**跟踪团队成员的任务状态。
- 主要优势 (Benefits)：
    - **改善团队沟通 (Improves communication)**。
    - **立即发现瓶颈 (Identifies bottlenecks immediately)**。
    - **鼓励团队自组织 (Encourages self-organization)**，无需项目经理干预。

### **16. 在制品 (WIP) 限制 (Work in Process (WIP) Limits)**

- **定义 (Definition)**：

    - **WIP 限制 (WIP limits)** 限制每列 (工作阶段) 允许的任务数量，以防止过载。

- **为什么要限制 WIP? (Why is WIP Limiting Important?)**

    - **减少交付时间 (Reduces Lead Time)**：从任务开始到完成的总时间减少。
    - **避免任务切换 (Prevents task switching)**：减少上下文切换，提高效率。
    - **促进团队合作 (Encourages teamwork)**：团队成员集中精力解决瓶颈，而非无序扩张任务。

- **Little’s Law (李特尔法则) 解释 WIP 限制的影响 (Little’s Law explains the relationship)**：
    $$
    {\text{Lead Time} = \frac{\text{WIP}}{\text{Cycle Time}}}
    $$

    - **降低 WIP 限制**，即可缩短交付时间。

- **Scrum vs. Kanban 的 WIP 管理 (Scrum vs. Kanban on WIP)**

    - **Scrum**：通过**Sprint 计划**隐式管理 WIP。
    - **Kanban**：**显式限制 WIP**，确保任务流动稳定。

### **17. Kanban 的约束管理 (Constraints in Kanban)**

- **WIP 限制暴露系统瓶颈 (WIP limits expose system constraints)**，显示任务积压的阶段。
- 当发现瓶颈时：
    - **团队停止拉取新任务 (The team stops pulling new work)**，直至问题解决。
    - **鼓励团队协作解决问题 (Encourages collaborative problem-solving)**。
- 与 Scrum 的不同 (Difference from Scrum)：
    - Scrum 通过**迭代回顾 (Retrospective) 解决问题**。
    - Kanban 采用**实时响应 (Real-time reaction)**，即刻处理瓶颈。

### **18. 流动性优先，而非资源利用率 (Flow Over Utilization)**

- 传统企业关注**资源利用率 (100% resource utilization)**，但 Kanban 强调**流动性 (Flow Efficiency)**。
- 核心概念 (Key concepts)：
    - **流动性效率比人员利用率更重要 (Flow efficiency is more important than keeping people constantly busy)**。
    - **专业化的一般主义者 (Specializing generalists)** 提高团队适应能力。

### **19. 变更管理与灵活性 (Planning for Variability)**

- **软件开发具有不确定性 (Software development is inherently variable)**，包括**任务周期长短不定, 需求变更**。
- Kanban 采用**拥抱变化 (Embraces variability)** 的方法，而不是试图消除它。
- 实现方式 (Approach)：
    - **使用小批量任务 (Use small batch sizes)**，便于应对需求变更。
    - **根据吞吐量动态调整 WIP 限制 (Adjust WIP limits based on observed throughput)**。

### **20. Kanban 是否需要估算和迭代? (No Estimates or Iterations?)**

- **Kanban 不依赖固定迭代 (Kanban doesn’t require fixed iterations)**，与 Scrum 的 Sprint 方式不同。
- **不依赖估算 (Does not rely on estimates)**：
    - 采用**跟踪交付时间 (Tracking Lead Time)** 进行预测。
    - **使用历史数据 (Uses historical data)** 计算交付节奏。
- **度量方式对比 (Comparison of Metrics)**：
    - **Scrum**：使用 **Velocity (速度)**，即每个 Sprint 交付的 Story Points。
    - **Kanban**：使用 **Lead Time (交付时间)**，即任务从开始到完成的平均时间。

### **21. 何时使用 Kanban? (When to Use Kanban?)**

- 适用于以下团队：
    - **工作负载高度变化 (Highly variable workloads)**。
    - **需要持续交付 (Continuous delivery)**，而非固定周期发布。
    - **希望优化效率 (Want to optimize efficiency)**，减少任务堆积。
- **Scrumban (Scrum + Kanban) 组合模式**：
    - 结合 Scrum 的**结构化规划** 和 Kanban 的**灵活流动**，平衡**可预测性和灵活性**。

### **22. 企业中的 Kanban (Kanban in the Enterprise)**

- **Kanban 具有可扩展性 (Kanban is scalable to large organizations)**，适用于跨部门、跨团队管理工作流。
- 在企业环境中的优势 (Benefits in enterprise settings)：
    - **提高可预测性 (Improves predictability)**，更容易规划交付时间。
    - **优化团队之间的依赖管理 (Helps manage dependencies between teams)**。
    - **加强跨业务部门协作 (Enhances collaboration across different business units)**。
