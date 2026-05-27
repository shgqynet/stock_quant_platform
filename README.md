# 量化选股分析平台

基于 Flask + ECharts 的 A 股量化分析工具，支持多策略回测、技术指标可视化、ATR 风控和参数优化。

## 功能特性

### 📊 实时行情
- 腾讯财经实时数据接口
- 搜索股票代码/名称
- 最新价、涨跌幅、开盘高开低收、成交量

### 📈 K线图表
- ECharts 交互式蜡烛图
- 可切换 6 种技术指标：MA5/10/20/60、布林带、MACD、RSI、KDJ
- 成交量副图
- 数据缩放/平移

### 🧠 四维评分系统
- **趋势分析** — 均线排列 + MACD 状态 + 趋势一致性（满分 30）
- **超买超卖** — KDJ + RSI 判断（满分 25）
- **量价分析** — 成交量形态 + OBV 资金流向（满分 25）
- **运动与位置** — 布林带位置 + 支撑阻力检测（满分 20）
- 综合评分自动产生信号：强买入/弱买入/观望/弱卖出/强卖出

### 🛡️ ATR 风控
- 自动计算 ATR(14) 波动率指标
- 动态止损价（2 倍 ATR）
- 动态止盈价（3 倍 ATR）

### 🔄 多策略回测
支持 4 种交易策略：

| 策略 | 说明 | 方向 |
|------|------|------|
| **MA5/MA20 金叉** | MA5 上穿 MA20 买入，下穿卖出 | 仅做多 |
| **Dual Thrust 通道突破** | 基于前 N 日 HH/LC 范围设定突破通道 | 多空双向 |
| **Heikin-Ashi 均值 K 线** | Heikin-Ashi 蜡烛图形态识别 | 多空双向 |
| **Parabolic SAR 抛物线** | 抛物线指标趋势跟踪，自动反转 | 多空双向 |

- ATR 动态止损止盈
- 多空双向交易支持
- 资金曲线、胜率、夏普比率、最大回撤、盈亏比等指标

### ⚡ 参数优化
- 网格搜索最优参数组合
- 以夏普比率为目标自动排序
- 展示 Top 10 参数排名

### 🔧 技术指标
MA / EMA / MACD / RSI / KDJ / BOLL / ATR / OBV / 支撑阻力

## 快速开始

### 环境要求
- Python 3.10+
- pip

### 安装

```bash
git clone https://github.com/yhwl8888/stock_quant_platform.git
cd stock_quant_platform
pip install -r requirements.txt
```

### 运行

```bash
python app.py
```

访问 `http://localhost:8000`

内网其他设备访问 `http://<本机IP>:8000`

### 使用

1. 在搜索框输入股票代码或名称（如 `600519` 或 `茅台`）
2. 点击「分析」或按回车
3. 查看实时行情、K 线图、综合评分
4. 切换技术指标标签
5. 选择策略并点击「运行回测」
6. 点击「参数优化」自动搜索最优参数

## API 接口

| 端点 | 方法 | 说明 |
|------|------|------|
| `/` | GET | 页面入口 |
| `/api/search?q=关键字` | GET | 搜索股票 |
| `/api/quote/<code>` | GET | 实时行情 |
| `/api/kline/<code>?days=250` | GET | K 线数据 |
| `/api/indicators/<code>` | GET | 技术指标 |
| `/api/analysis/<code>` | GET | 综合评分 |
| `/api/backtest` | POST | 策略回测 |
| `/api/optimize` | POST | 参数优化 |
| `/api/strategies` | GET | 策略列表 |
| `/api/support-resistance/<code>` | GET | 支撑阻力 |

## 参数优化参考（茅台 600519，120 天）

### Dual Thrust 最优参数
| 参数 | 值 |
|------|------|
| K 值 | 0.7 |
| 持有天数 | 5 |
| 止损比例 | 3% |
| 止盈比例 | 8% |
| **夏普比率** | **1.61** |
| **收益率** | **+10.47%** |

## 数据说明

- 数据来源：腾讯财经（免费接口）
- 支持：上海（sh）、深圳（sz）、北京（bj）市场
- 注意：北交所（bj）股票暂不支持 K 线数据
- 本工具仅用于学习参考，不构成投资建议

## 项目结构

```
stock_quant_platform/
├── app.py                 # Flask 主服务
├── config.py              # 配置
├── requirements.txt       # Python 依赖
├── data/
│   ├── fetcher.py         # 数据获取（腾讯财经 API）
│   └── utils.py           # 股票代码解析
├── engine/
│   ├── indicators.py      # 技术指标
│   ├── scoring.py         # 四维评分
│   ├── backtest.py        # 回测引擎
│   ├── strategies.py      # 交易策略
│   └── optimizer.py       # 参数优化
├── static/
│   ├── css/style.css      # 样式（深色玻璃拟态）
│   └── js/app.js          # 前端逻辑
└── templates/
    └── index.html         # 单页模板
```

## 许可证

MIT
