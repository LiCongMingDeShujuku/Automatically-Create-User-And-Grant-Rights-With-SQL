![CLEVER DATA GIT REPO](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/0-clever-data-github.png "李聪明的数据库")

# 自动创建用户和授权SQL
#### Automatically Create User And Grant Rights
**发布-日期: 2014年06月12日 (评论)**

## Contents

- [中文](#中文)
- [English](#English)
- [SQL Logic](#Logic)
- [Build Info](#Build-Info)
- [Author](#Author)
- [License](#License) 


## 中文
这是一个快速脚本，它将创建一个登录，并自动将其添加到每个数据库（通过你指定的通配符），并授予两个只读的角色，并拒绝编写。

该脚本执行以下操作：
1，检查用户是否存在于主数据库中。 
2，如果用户尚不存在，则创建用户。
3，检查每个数据库中是否存在相同的用户。 
4，如果用户尚不存在，则创建用户
5，将用户添加到角色db_datareader和db_denydatawriter。

注意：
如果用户已经存在，它将不执行任何操作。

运行脚本：


## English
Here’s a quick script that will create a login, and automatically add it to every database ( specified through a wild card of your choice ), and grant two roles read only, and deny writer.
The script does the following:
1. Checks to see if the User exists in the Master database. 2. Creates the User if it does NOT already exists.
3. Checks to see if the same user exists in each database. 4. Creates the User if it does NOT already exist
5. Adds the User to the roles db_datareader, and db_denydatawriter.
Note:
If the user already exists it will do nothing.
On with the script…


---
## Logic
```SQL
use master;
set nocount on
 
if not exists( select name from syslogins where name = 'My_SQL_Login' ) begin
create login [LinkServ]
with
password = N'MyPassword' , default_database = [master] , check_expiration = off , check_policy = off end
 
declare @create_user_grant_rights varchar(max)
set @create_user_grant_rights = ''
select @create_user_grant_rights = @create_user_grant_rights + 'use [' + name + '];' + char(10) +
'if not exists ( select name from sysusers where name = ''My_SQL_Login'' )' + char(10) +
'begin' + char(10) +
'create user [My_SQL_Login] for login [My_SQL_Login]' + char(10) +
'exec sp_addrolemember N''db_datareader'', N''My_SQL_Login''' + char(10) +
'exec sp_addrolemember N''db_denydatawriter'', N''My_SQL_Login''' + char(10) +
'end' + char(10) from sys.databases where name like '%wildcard%'
exec (@create_user_grant_rights)


```


[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

---

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

- **李聪明的数据库 Lee's Clever Data**
- **Mike的数据库宝典 Mikes Database Collection**
- **李聪明的数据库** "Lee Songming"

[![Gist](https://img.shields.io/badge/Gist-李聪明的数据库-<COLOR>.svg)](https://gist.github.com/congmingshuju)
[![Twitter](https://img.shields.io/badge/Twitter-mike的数据库宝典-<COLOR>.svg)](https://twitter.com/mikesdatawork?lang=en)
[![Wordpress](https://img.shields.io/badge/Wordpress-mike的数据库宝典-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

---
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Lee Songming](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/1-clever-data-github.png "李聪明的数据库")

