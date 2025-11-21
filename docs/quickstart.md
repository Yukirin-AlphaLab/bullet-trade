# 快速上手：三步跑通（聚宽兼容）

目标：用原有聚宽策略，最少改动跑通回测与实盘（本地或远程）。如需细节配置，参考 [配置总览](config.md)。

## 1. 安装与准备环境
```bash
python -m venv .venv && source .venv/bin/activate
pip install -e "bullet-trade[dev]"
```
复制模板：
```bash
cp env.backtest.example .env    # 回测
cp env.live.example .env        # 实盘/远程
```

## 2. 兼容聚宽的最小策略
```python
from jqdata import *

def initialize(context):
    set_benchmark('000300.XSHG')
    set_option('use_real_price', True)
    g.stocks = ['000001.XSHE', '600000.XSHG']
    run_daily(trade, time='10:00')

def trade(context):
    order(g.stocks[0], 100)
    order(g.stocks[1], 200)
```
> 聚宽 API 直接可用，无需改接口；仅通过 `.env` 切换数据源/券商。

## 3. 直接运行
- 回测：
```bash
bullet-trade backtest strategies/demo_strategy.py --start 2025-01-01 --end 2025-06-01
```
- 本地实盘（QMT/模拟）：
```bash
bullet-trade live strategies/demo_strategy.py --broker qmt
```
- 远程实盘（qmt-remote）：
```bash
bullet-trade live strategies/demo_strategy.py --broker qmt-remote
```

## 我应该看哪篇文档？
- 配置说明：查看 [配置总览](config.md)
- 回测细节/FAQ：看 [回测引擎](backtest.md)
- 本地/远程实盘：看 [实盘引擎](live.md)
- 远程 QMT server 部署：看 [QMT server](qmt-server.md)
- 聚宽模拟盘接入远程实盘：看 [trade-support](trade-support.md)
