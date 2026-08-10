[C# 图书管理系统 winform 入门教程 SqlServer数据库_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV14p4y1S7yk/?spm_id_from=333.337.search-card.all.click&vd_source=e7e5c9034979528d120ab89a1e74801a)
# 效果
![[Pasted image 20260810195438.png|293]]
![[Pasted image 20260810195508.png|355]]
![[Pasted image 20260810195638.png|333]]
![[Pasted image 20260810200803.png|165]]
![[Pasted image 20260810200750.png|167]]

# 界面
- PS:
	- 单选，设置为默认true;
- Data.cs 存储登录数据；
- Dao.cs 对数据库进行操作；

- Dao.cs
```Csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace BookMES
{
    class Data
    {
        public static string UID = "", UName = "";//登录用户的ID和用户名

    }
}
```

![[Pasted image 20260810202743.png]]


# SSMS+SQL Server
- SSMS(SQL Server Management Studio)
	- [安装 SQL Server Management Studio | Microsoft Learn](https://learn.microsoft.com/zh-cn/ssms/install/install)
- 微软SQL Server:[SQL Server 2022 | Microsoft](https://www.microsoft.com/zh-cn/sql-server/sql-server-2022)

![[Pasted image 20260810213153.png]]- C:\Program Files\Microsoft SQL Server\160\Setup Bootstrap\Log\20260810_212849
![[Pasted image 20260810213435.png|228]]
