# 山村预警送达时限的争议复核

这些演练台账是本地合成资料，使用虚构编号。CSV 采用 UTF-8 和带表头的逗号分隔格式。同一单元内用竖线列出的值表示多项或候选集合，含义见各表说明。空值通常表示未记录或不适用，明确的零会保留为 0。

## 资料

- `data/contracts.csv`  有效范围内的送达约定。minutes 从对该村有效的实质义务签发时起算。dispute_deadline 为轮次首日起算的最后可送出异议日。
  字段为 `party / villages / minutes / dispute_deadline / proof`。
- `data/clocks.csv`  偏移范围给出由报送时间换算为本地真实事件分钟时需加的分钟。可接受 sources 表示记录来源，不表示独立。
  字段为 `device / source / min_offset / max_offset`。
- `data/issues.csv`  period 为预警轮次，version 的 prev 只关联同一轮旧稿。change 为该次改动类型。covered 记录生效村，owner 是签发台。
  字段为 `period / version / prev / minute / covered / change / owner`。
- `data/handoffs.csv`  accepted_minute 是接收方取得稿件和处置资格的时间。村的独立截止由 contracts 解释。
  字段为 `period / version / from_party / to_party / accepted_minute / scope`。
- `data/receipts.csv`  多个抄录可能具有共同的 root。station 类型只证明到站，audience 类型表示记录人员实际接触内容。scope 是可能对应的村；copy_count 是该回执最多能够独立证明的村数。
  字段为 `id / period / candidates / minute / device / scope / root / kind / copy_count / received_day`。
- `data/observed_order.csv`  复核能够独立确认的顺序。late correction 不覆盖原始记录。
  字段为 `period / earlier / later / received_day`。
- `data/disputes.csv`  已登记的异议和待补查动作的业务状态。scope 表示经办人当时采用的村和稿件范围。
  字段为 `case / period / party / day / state / scope`。

## 核对台账

环境为 Python 3.11.2，只使用标准库。在仓库根目录运行下列命令检查文件格式、表头和基础外键。输出为各张源表可读行数。

```sh
python3 input_audit.py
python3 -m unittest discover -s tests -v
```

可在第一条命令后指定另一个输入目录，目录中的同名 CSV 沿用上述格式。列名和基础引用清单见 `input_contract.json`。
