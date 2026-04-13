![Python](https://img.shields.io/badge/python-3.8%2B-blue)
![Pandas](https://img.shields.io/badge/pandas-1.x-orange)
![MySQL](https://img.shields.io/badge/mysql-8.x-blue)
![License](https://img.shields.io/badge/license-MIT-green)

# Olist 电商客户分层分析 (RFM)

本项目利用 Python 处理巴西电商 Olist 的 10 万+ 条真实订单数据，通过 RFM 模型实现客户价值分层。

数据架构：本项目采用 MySQL 作为底层数据仓储，存储了包含客户、订单、支付等多张关联表的原始电商数据（10万+记录）。通过 Python 与数据库的联动，实现了从数据持久化层到分析层的无缝衔接。

数据来源：https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce

数据库连接管理：使用 SQLAlchemy 库构建数据库引擎（Engine），并通过 mysql-connector-python 驱动实现与本地 MySQL 服务器的高效通信。

复杂 ETL 处理：编写 SQL 语句进行多表联查与初步清洗，确保进入 Pandas 环境的数据具有高度的关联性与准确性。

数据回传与持久化：分析完成后，利用 to_sql 方法将计算出的 RFM 分值及客户分层标签（Customer Tag）重新存入数据库，通过设置 chunksize 优化了海量数据的写入性能，确保了分析结果的可追溯性。

## 🗄️ 数据库集成 (MySQL Integration)

本项目不仅是一个数据分析实验，更是一个完整的数据库应用案例。

### 1. 数据库环境
- **数据库类型**: MySQL 8.x
- **连接方式**: SQLAlchemy + mysql-connector-python
- **原始数据集**: `olist_db` (包含 orders, order_items, customers 等 7 张核心表)

### 2. 核心 SQL 与处理流程
- **提取 (Extract)**: 编写 SQL 聚合查询，提取客户 ID、订单时间及支付金额。
- **转换 (Transform)**: 在 Python 环境中利用 Pandas 处理长尾分布数据，通过 `rank` 辅助 `qcut` 完成打分。
- **加载 (Load)**: 将带有分析标签的最终数据回传至 MySQL，表名为 `rfm_segmentation_result`。

### 3. 如何配置数据库
若要复现本项目，请确保本地 MySQL 已运行并创建数据库：
```sql
CREATE DATABASE olist_db;
-- 导入原始数据集后，在 Notebook 中修改连接字符串：
-- engine = create_engine('mysql+mysqlconnector://root:密码@127.0.0.1:3306/olist_db')

