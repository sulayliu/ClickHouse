---
machine_translated: true
machine_translated_rev: 5decc73b5dc60054f19087d3690c4eb99446a6c3
---

# system.mutations {#system_tables-mutations}

该表包含以下信息 [mutations](../../sql-reference/statements/alter.md#alter-mutations) MergeTree表及其进展。 每个mutation命令由一行表示。 该表具有以下列:

**database**, **table** -应用mutation的数据库和表的名称。

**mutation_id** -mutation的ID 对于replicated的表，这些Id对应于znode中的名称 `<table_path_in_zookeeper>/mutations/` zookeeper的目录。 对于non-replicated的表，Id对应于表的数据目录中的文件名。

**command** -Mutation命令字符串（查询后的部分 `ALTER TABLE [db.]table`).

**create_time** -这个mutation命令被提交执行时间。

**block_numbers.partition_id** (Array(String)) - 对于replicated的mutation，数组包含分区的 ID（每个分区一个记录）；对于non-replicated的mutation，数组为空。 

**block_numbers.number** (Array(Int64)) - 对于replicated的mutation，数组包含每个分区的一条记录，其中包含mutation获得的块号。 只有包含数量小于此数量的块的部分才会在分区中发生变异。在non-replicated表中，所有分区中的块号形成一个序列。 这意味着对于non-replicated表的mutation，该列将包含一条记录，该记录具有由mutation获得的单个块号。

**parts_to_do_names** (Array(String)) - 需要进行mutation以完成mutation的数据部分的名称数组。

**parts_to_do** (Int64) — 完成mutation需要mutation的数据部分的数量。

**is_done** (UInt8) — mutation是否完成的标志。 可能的值：

  1 突变完成，
  0 突变仍在进行中。
如果更改某些数据部分时出现问题，以下列包含附加信息：



如果在改变某些部分时出现问题，以下列将包含其他信息:

**latest_failed_part** (String) — 无法更改的最新部分的名称。

**latest_fail_time** (Datetime) — 最近部分mutation失败的日期和时间。

**latest_fail_reason** (String) — 导致最近部分mutation失败的异常消息。
