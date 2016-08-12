# db_account
##### `t_account`
```
CREATE TABLE `t_account` (
  `id` int(11) unsigned NOT NULL auto_increment,
  `email` varchar(128) NOT NULL default '',
  `password` varchar(32) NOT NULL default '',
  `email_password` varchar(32) NOT NULL default '' COMMENT '邮箱密码',
  `salt` char(6) NOT NULL,
  `gender` char(1) NOT NULL default 'F',
  `nickname` varchar(32) NOT NULL default '',
  `realname` varchar(32) NOT NULL default '',
  `birthday` int(8) NOT NULL default '0',
  `province` int(11) NOT NULL default '0',
  `city` int(11) NOT NULL default '0',
  `county` int(11) NOT NULL default '0',
  `mobile_no` varchar(16) NOT NULL default '',
  `addrinfo` text NOT NULL,
  `address` varchar(255) NOT NULL default '',
  `zip_code` varchar(8) NOT NULL default '',
  `home_phone` varchar(16) NOT NULL default '',
  `office_phone` varchar(16) NOT NULL default '',
  `avatar` varchar(64) NOT NULL default '',
  `active_code` varchar(32) default NULL,
  `role` char(1) NOT NULL default 'P' COMMENT '用户身份信息: M 母亲, F 父亲, C 孩子, P 父母',
  `step` tinyint(1) NOT NULL default '0' COMMENT '注册引导页页面下标, 从1开始, 为0表示已经完成引导',
  `safe_question` text COMMENT '安全问题及答案,存入时序列化，取出时反序列化',
  `last_login_time` int(11) NOT NULL default '0' COMMENT '上次登录时间',
  `update_time` int(11) NOT NULL default '0',
  `create_time` int(11) NOT NULL default '0',
  `status` char(1) NOT NULL COMMENT '账号状态取值: U 未激活, Y 已激活, N 禁用',
  `verify_state` tinyint(1) NOT NULL default '0' COMMENT '身份验证状态: 0 未验证, 1 email验证, 2 手机验证, 3 email+手机',
  `verify_code` varchar(32) NOT NULL default '' COMMENT '身份验证码',
  `login_mobile_no` varchar(16) NOT NULL default '' COMMENT '用作登陆名的手机号码',
  `login_by_mobile_no` tinyint(4) NOT NULL default '0' COMMENT '是否可以通过手机号码登陆',
  PRIMARY KEY  (`id`),
  KEY `email` (`email`),
  KEY `active_code` (`active_code`),
  KEY `nickname` (`nickname`),
  KEY `mobile_no` (`mobile_no`),
  KEY `login_mobile_no` (`login_mobile_no`,`login_by_mobile_no`)
) ENGINE=MyISAM AUTO_INCREMENT=4859194 DEFAULT CHARSET=utf8;
```
##### `t_child`
```
CREATE TABLE `t_child` (
  `id` int(11) unsigned NOT NULL auto_increment,
  `nickname` varchar(32) NOT NULL default '' COMMENT '宝宝昵称/小名',
  `realname` varchar(32) NOT NULL default '' COMMENT '宝宝真实姓名',
  `gender` char(1) NOT NULL default 'M' COMMENT '宝宝性别',
  `avatar` varchar(64) NOT NULL default '',
  `birthday` int(11) NOT NULL default '0' COMMENT '宝宝生日',
  `birthday_flag` tinyint(1) NOT NULL default '0' COMMENT '生日标志位, 按bit存储, 用于支持农历/模糊年龄',
  `owner_uid` int(11) NOT NULL default '0' COMMENT '创建的父母id',
  `update_time` int(11) NOT NULL default '0',
  `create_time` int(11) NOT NULL default '0',
  `status` enum('Y','N') NOT NULL default 'Y',
  `child_type` tinyint(1) NOT NULL default '0' COMMENT '宝宝类型，0:已出生宝宝, 1:孕期宝宝, 2:其它',
  PRIMARY KEY  (`id`),
  KEY `nickname` (`nickname`),
  KEY `owner` (`owner_uid`)
) ENGINE=InnoDB AUTO_INCREMENT=2002225 DEFAULT CHARSET=utf8;
```
##### `t_common_task`
```
CREATE TABLE `t_common_task` (
  `id` smallint(6) unsigned NOT NULL auto_increment,
  `pf` tinyint(1) unsigned NOT NULL default '0' COMMENT '平台代码',
  `name` varchar(50) NOT NULL default '' COMMENT '任务名称',
  `description` text NOT NULL COMMENT '任务描述',
  `icon` varchar(150) NOT NULL default '' COMMENT '任务图标',
  `url` varchar(512) NOT NULL default '' COMMENT '任务参加地址',
  `applicants` mediumint(8) unsigned NOT NULL default '0' COMMENT '已申请任务人次',
  `achievers` mediumint(8) unsigned NOT NULL default '0' COMMENT '已完成任务人次',
  `limits` mediumint(8) unsigned NOT NULL default '0' COMMENT '允许申请并完成任务的人次上限',
  `reward` enum('credit','medal') NOT NULL default 'credit' COMMENT '任务奖励类型, credit:积分, medal:勋章',
  `prize` varchar(15) NOT NULL default '' COMMENT '任务奖品(积分规则ID, 勋章种类ID)',
  `bonus` int(11) NOT NULL default '1' COMMENT '奖品数量/有效期',
  `show_order` smallint(2) unsigned NOT NULL default '0' COMMENT '展示顺序',
  `start_time` int(11) unsigned NOT NULL default '0' COMMENT '任务开始时间',
  `end_time` int(11) unsigned NOT NULL default '0' COMMENT '任务结束时间',
  `url_apply` varchar(255) NOT NULL default '' COMMENT '任务参与地址',
  `status` tinyint(1) unsigned NOT NULL default '0' COMMENT '任务状态, 0正常, 1不启用 99删除',
  PRIMARY KEY  (`id`),
  KEY `pf` (`pf`)
) ENGINE=MyISAM AUTO_INCREMENT=6 DEFAULT CHARSET=utf8;
```
##### `t_credit_log`
```
CREATE TABLE `t_credit_log` (
  `id` int(11) unsigned NOT NULL auto_increment,
  `uid` int(11) unsigned NOT NULL default '0' COMMENT '用户ID',
  `operation` tinyint(2) unsigned NOT NULL default '0' COMMENT '操作类型',
  `relatedid` int(11) unsigned NOT NULL default '0' COMMENT '操作相关ID',
  `related_desc` varchar(64) NOT NULL default '' COMMENT '操作相关描述',
  `credits1` int(11) NOT NULL default '0' COMMENT '扩展积分1',
  `credits2` int(11) NOT NULL default '0' COMMENT '扩展积分2',
  `credits3` int(11) NOT NULL default '0' COMMENT '扩展积分3',
  `ctime` int(11) unsigned NOT NULL default '0' COMMENT '记录时间',
  PRIMARY KEY  (`id`),
  KEY `uid` (`uid`),
  KEY `operation` (`operation`)
) ENGINE=MyISAM AUTO_INCREMENT=9948 DEFAULT CHARSET=utf8;
```
##### `t_credit_rule`
```
CREATE TABLE `t_credit_rule` (
  `id` mediumint(8) unsigned NOT NULL auto_increment,
  `rule_name` varchar(20) NOT NULL default '' COMMENT '规则名称',
  `action` varchar(20) NOT NULL default '' COMMENT '对应的动作',
  `cycle_type` tinyint(1) NOT NULL default '0' COMMENT '奖励周期, 0:一次, 1:每天, 2:整点, 3:间隔分钟, 4:不限',
  `cycle_time` int(11) NOT NULL default '0' COMMENT '奖励间隔时间',
  `rewardnum` tinyint(2) NOT NULL default '1' COMMENT '奖励次数',
  `norepeat` tinyint(1) NOT NULL default '0' COMMENT '是否去重, 0:不去重, 1去重',
  `credits1` int(11) NOT NULL default '0' COMMENT '扩展积分1',
  `credits2` int(11) NOT NULL default '0' COMMENT '扩展积分2',
  `credits3` int(11) NOT NULL default '0' COMMENT '扩展积分3',
  PRIMARY KEY  (`id`),
  UNIQUE KEY `action` (`action`)
) ENGINE=MyISAM AUTO_INCREMENT=6 DEFAULT CHARSET=utf8;
```
##### `t_credit_rule_log`
```
CREATE TABLE `t_credit_rule_log` (
  `id` int(11) unsigned NOT NULL auto_increment,
  `uid` int(8) unsigned NOT NULL default '0' COMMENT '用户ID',
  `rid` mediumint(8) unsigned NOT NULL default '0' COMMENT '规则ID',
  `total` mediumint(8) unsigned NOT NULL default '0' COMMENT '规则被执行总次数',
  `cycle_num` mediumint(8) unsigned NOT NULL default '0' COMMENT '周期被执行次数',
  `credits1` int(11) NOT NULL default '0' COMMENT '扩展积分1',
  `credits2` int(11) NOT NULL default '0' COMMENT '扩展积分2',
  `credits3` int(11) NOT NULL default '0' COMMENT '扩展积分3',
  `start_time` int(11) unsigned NOT NULL default '0' COMMENT '周期开始时间',
  `last_time` int(11) unsigned NOT NULL default '0' COMMENT '规则最后执行时间',
  `ctime` int(11) unsigned NOT NULL default '0' COMMENT '记录时间',
  PRIMARY KEY  (`id`),
  KEY `uid` (`uid`,`rid`),
  KEY `last_time` (`last_time`)
) ENGINE=MyISAM AUTO_INCREMENT=1751 DEFAULT CHARSET=utf8;
```
##### `t_email`
```
CREATE TABLE `t_email` (
  `id` int(11) unsigned NOT NULL auto_increment,
  `uid` int(11) NOT NULL default '0',
  `old_email` varchar(128) NOT NULL,
  `new_email` varchar(128) NOT NULL,
  `active_code` varchar(32) NOT NULL default '',
  `update_time` int(11) NOT NULL default '0',
  `create_time` int(11) NOT NULL default '0',
  `status` char(1) NOT NULL COMMENT '状态取值: Y 有效, N 无效',
  PRIMARY KEY  (`id`),
  KEY `uid` (`uid`,`old_email`,`new_email`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8;
```
##### `t_lenovo_users`
```
CREATE TABLE `t_lenovo_users` (
  `id` int(11) NOT NULL auto_increment,
  `lenovo_key` varchar(64) NOT NULL default '',
  `uid` int(11) NOT NULL default '0',
  `create_time` datetime NOT NULL default '0000-00-00 00:00:00',
  `IP` varchar(15) NOT NULL default '0.0.0.0',
  PRIMARY KEY  (`id`)
) ENGINE=MyISAM AUTO_INCREMENT=18 DEFAULT CHARSET=utf8;
```
##### `t_mytask`
```
CREATE TABLE `t_mytask` (
  `id` int(11) unsigned NOT NULL auto_increment,
  `uid` int(11) unsigned NOT NULL,
  `taskid` smallint(6) unsigned NOT NULL,
  `progress` char(255) NOT NULL default '' COMMENT '任务进度',
  `ctime` int(11) unsigned NOT NULL default '0' COMMENT '任务申请时间',
  `status` tinyint(1) NOT NULL default '0' COMMENT '任务状态, 0进行中, 1完成, 2失败',
  PRIMARY KEY  (`id`),
  KEY `user_task` (`uid`,`taskid`)
) ENGINE=MyISAM AUTO_INCREMENT=2243 DEFAULT CHARSET=utf8;
```
##### `t_open_bind`
```
CREATE TABLE `t_open_bind` (
  `id` int(11) unsigned NOT NULL auto_increment,
  `open_platform` tinyint(1) NOT NULL default '0' COMMENT '开放平台编号',
  `open_id` varchar(32) NOT NULL default '' COMMENT '开放平台账号唯一标识',
  `uid` int(11) NOT NULL default '0' COMMENT '我们网站的uid',
  `open_nickname` varchar(32) NOT NULL default '' COMMENT '开放平台账号昵称',
  `access_token` varchar(128) NOT NULL default '',
  `refresh_token` varchar(128) NOT NULL default '',
  `open_data` varchar(512) NOT NULL default '' COMMENT '开放平台账号的session数据, 以json格式存储',
  `bind_time` int(11) NOT NULL default '0',
  `status` enum('Y','N') NOT NULL default 'Y' COMMENT '状态取值: Y 有效, N 无效',
  PRIMARY KEY  (`id`),
  KEY `open` (`open_id`,`open_platform`),
  KEY `uid` (`uid`)
) ENGINE=InnoDB AUTO_INCREMENT=945366 DEFAULT CHARSET=utf8;
```
##### `t_relation`
```
CREATE TABLE `t_relation` (
  `id` int(11) unsigned NOT NULL auto_increment,
  `parent_uid` int(11) NOT NULL default '0',
  `child_uid` int(11) NOT NULL default '0',
  `relation_type` tinyint(1) NOT NULL default '0',
  `role_id` tinyint(1) NOT NULL default '0',
  `role_name` varchar(16) NOT NULL default '',
  `update_time` int(11) NOT NULL default '0',
  `create_time` int(11) NOT NULL default '0',
  `status` char(1) NOT NULL COMMENT '状态取值: Y 有效, N 无效',
  PRIMARY KEY  (`id`),
  KEY `parent` (`parent_uid`,`child_uid`),
  KEY `child` (`child_uid`,`relation_type`)
) ENGINE=MyISAM AUTO_INCREMENT=1629843 DEFAULT CHARSET=utf8;
```
##### `t_user_address`
```
CREATE TABLE `t_user_address` (
  `id` int(11) NOT NULL auto_increment,
  `uid` int(11) NOT NULL default '0' COMMENT '账号id',
  `name` varchar(32) NOT NULL default '' COMMENT '收货人',
  `province` int(11) NOT NULL default '0' COMMENT '省份id',
  `city` int(11) NOT NULL default '0' COMMENT '城市id',
  `county` int(11) NOT NULL default '0' COMMENT '县级id',
  `street` varchar(255) NOT NULL default '' COMMENT '街道信息',
  `mobile` char(11) NOT NULL default '' COMMENT '手机号码',
  `is_default` tinyint(4) NOT NULL default '0' COMMENT '是否是默认地址',
  `mark` varchar(255) NOT NULL default '' COMMENT '标记名',
  `ctime` int(10) unsigned NOT NULL default '0',
  `mtime` int(10) unsigned NOT NULL default '0',
  `status` tinyint(4) NOT NULL default '0',
  PRIMARY KEY  (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8;
```
##### `t_vip_score`
```
CREATE TABLE `t_vip_score` (
  `uid` int(11) NOT NULL default '0',
  `score` int(11) NOT NULL default '0',
  `last_update_time` int(11) NOT NULL default '0',
  `create_time` datetime NOT NULL default '0000-00-00 00:00:00',
  PRIMARY KEY  (`uid`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8;
```
##### `t_vip_score_detail`
```
CREATE TABLE `t_vip_score_detail` (
  `id` int(11) NOT NULL auto_increment,
  `uid` int(11) NOT NULL default '0',
  `score` int(11) NOT NULL default '0',
  `channel` int(11) NOT NULL default '0',
  `remark` varchar(255) NOT NULL default '',
  `create_time` datetime NOT NULL default '0000-00-00 00:00:00',
  PRIMARY KEY  (`id`)
) ENGINE=MyISAM AUTO_INCREMENT=791473 DEFAULT CHARSET=utf8;
```

