# pkg_config

|          |                          |
| :------- | :----------------------- |
| **函数** | `pkg_config`             |
| **目的** | 运行任何特殊的安装后配置 |
| **沙盒** | 不具备                   |
| **权限** | root                     |
| **调用** | 手册                     |

## 默认`pkg_config`

```bash
pkg_config()
{
	eerror "This ebuild does not have a config function."
}
```

## `pkg_config`样例

取自 `mysql`ebuild。注意`${ROOT}`的使用。

```bash
pkg_config() {
	if [ ! -d "${ROOT}"/var/lib/mysql/mysql ] ; then
		einfo "Press ENTER to create the mysql database and set proper"
		einfo "permissions on it, or Control-C to abort now..."
		read
		"${ROOT}"/usr/bin/mysql_install_db
	else
		einfo "Hmm, it appears as though you already have the mysql"
		einfo "database in place.  If you are having problems trying"
		einfo "to start mysqld, perhaps you need to manually run"
		einfo "/usr/bin/mysql_install_db and/or check your config"
		einfo "file(s) and/or database(s) and/or logfile(s)."
	fi
}
```
