# 从Git到RSYNC

对tree所做的更改将分阶段传播给用户：

开发人员致力于本地Git克隆并推送到中央远程Git存储库。使用基于GPG的Git机制对提交和推送进行签名。
暂存盒从中央Git存储库同步，从Git历史记录生成元数据缓存，ChangeLog和签名的清单。
rsync1 从登台框同步。
公共rsync服务器从同步rsync1。
用户从公共rsync服务器同步。
Git到RSYNC传播
该图显示了Git到RSYNC的传播

该emerge-websync快照从分段箱每天现做。



