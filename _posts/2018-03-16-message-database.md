---
layout: post
title: 消息管理数据字典设计
description: ""
tags: [database, message]
image:
  background: abstract-12.jpg
---

### 消息管理数据字典

#### notify
	字段名|字段描述|类型|可否为空|备注
	-----|-----|-----|-----|-----
	id|主键|int64|否|-
	title|标题|varchar(16)|否|
	second_title|二级标题|varchar(100)|是|
	content|内容|text|否|
	template_id|模板ID|int(11)|是|默认0
	type|类型|tinyint(1)|否|1：公告 2：提醒
	target|目标的ID|int11|是|预留字段
	target_type|目标的类型|varchar(30)|是|预留字段
	action|动作|varchar(30)|是|预留字段
	extra_field|扩展字段|json|是|预留字段
	send_method|发送方式|tinyint(1)|否|1:push 2:短信 3:站内信
	scheduled|是否计划发送|tinyint(1)|否|0:立即发送 1:计划发送
	scheduled_send_time|计划发送的时间|timestamp|是|
	created_at|创建时间|timestamp|否|记录创建时自动生成
	updated_at|更新时间|timestamp|否|记录修改是自动更新

#### notify_user
	字段名|字段描述|类型|可否为空|备注
	-----|-----|-----|-----|-----
	id|主键|int64|否|-
	is_read|是否已读|tinyint|否|0:未读 1:已读
	member_id|用户ID|int11|否|
	notify_id|消息ID|int11|否|
	created_at|创建时间|timestamp|否|记录创建时自动生成
	updated_at|更新时间|timestamp|否|记录修改是自动更新

#### notify_template
	字段名|字段描述|类型|可否为空|备注
	-----|-----|-----|-----|-----
	id|主键|int64|否|-
	name|模板名称|varchar(255)|否|中文
	slug|模板别名|varchar(255)|否|仅限英文，方便代码调用
	title|标题|varchar(16)|否|
	second_title|二级标题|varchar(100)|是|
	content|内容|text|否|
	status|状态|tinyint|否|0：关闭 1：开启
	remark|备注|text|是|预留字段
	created_at|创建时间|timestamp|否|记录创建时自动生成
	updated_at|更新时间|timestamp|否|记录修改是自动更新


> notify表存储公告跟提醒消息，notify_user表关联notify表。比如后台发布了一条公告，那么notify表就会有一条公告数据，notify_user表对应所发送的用户。发送所有用户，notify_user表就存储所有用户的用户ID跟对应的消息ID，发送特定用户或者个人用户就存储所对应的消息ID和用户ID。

* 事例

#
	notify

	id：1
	title：测试公告
	second_title：二级标题
	content：内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容
	template_id：0
	type：1
	send_type：1


#
	notify_user

	is_read：0
	member_id：1
	notify_id：1


> notify_template表是存储消息模板。假设notify_template有一条模板数据并且已经开启。用户申请贷款成功后发送提醒消息。

* 事例
#
	notify_template

	id：1
	name：贷款消息模板
	slug：msg_loan
	title：测试模板1
	second_title：二级标题
	content：尊敬的客户，您已申请{$productName}（产品名称）贷款，一共贷款 {$money}元，贷款期限为{$month}月，银行工作人员将于15个工作日内联系你审核资料，感谢您的支持。
	status：1

#
	notify

	id：2
	title：测试模板1
	second_title：二级标题
	content：尊敬的客户，您已申请邮储银行-xxx贷款，一共贷款 30000元，贷款期限为12月，银行工作人员将于15个工作日内联系你审核资料，感谢您的支持。
	template_id：1
	type：2
	send_type：3


#
	notify_user

	is_read：0
	member_id：1
	notify_id：2
