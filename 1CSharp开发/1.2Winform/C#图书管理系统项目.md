[C# 图书管理系统 winform 入门教程 SqlServer数据库_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV14p4y1S7yk/?spm_id_from=333.337.search-card.all.click&vd_source=e7e5c9034979528d120ab89a1e74801a)

# 架构(三层架构Three-Tier Architecture+DAO(Data Access Object))

|                      |                            |                                           |                                              |
| -------------------- | -------------------------- | ----------------------------------------- | -------------------------------------------- |
| 表现层UI                | Form1.cs/Form1.Designer.cs | 用户界面展示与交互                                 |                                              |
| 数据访问层DAL             | Dao.cs                     | 封装所有数据库操作(连接、增删改查)                        |                                              |
| 数据/状态层(全局状态类、会话上下文类) | Data.cs                    | 存储全局共享数据(当前登录用户的ID和用户名)；作用是存储当前登录用户的全局信息。 | static，整个程序都可以通过Data.UID和Data.UName访问当前谁在操作。 |
| 入口                   | Program.cs                 | 程序启动入口                                    |                                              |
|                      |                            |                                           |                                              |
- DAO模式:
	- Dao.cs是典型的数据访问对象，把数据库的底层操作(连接、命令、执行、读取、关闭)全部封装在一个类中。
	- UI层(Form1.cs)需要操作数据时，只需要调用Dao的方法并传入SQL语句即可，不需要关心连接怎么打开，怎么关闭。
	- 标准三层结构
		- [Winform的三层架构详细案例分析 - 码农阿亮 - 博客园](https://www.cnblogs.com/wml-it/p/15967883.html)
		- ![[Pasted image 20260816062620.png|247]]
			- **界面层 (UI)**: 负责用户交互和界面展示。
			- **业务逻辑层 (BLL)**: 处理核心业务规则和逻辑。
			- **数据访问层 (DAL)**: 负责与数据库进行所有交互。
			- **实体类 (Entity/Model)**: 作为数据载体，在各层之间传递数据，通常与数据库表结构一一对应。
		- 标准:UI+BLL+DAL+Entity;简化项目:UI+DAO![[Pasted image 20260816062951.png]]



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

  
# √1-登陆页面设计和数据库设计
- 登录页面设计

- 数据库表
![[Pasted image 20260816064530.png]]




# 2-登陆功能的实现
- 2个单选按钮
	- 改名
		- ![[Pasted image 20260816065853.png|243]]
		- ![[Pasted image 20260816065805.png|246]]
	- Form1.cs
```CSharp
  //登录，验证是否允许登录，允许返回true
  public Boolean login()
  {
      //用户
      if (radioButtonUser.Checked == true)
      {
          Dao dao = new Dao();
          //3种方式
          string sql = "select * from t_user where id='" + textBox1.Text + "' and psw='" + textBox2.Text + "'";
          ////使用 {0} 和 {1} 作为占位符，依次替换为后面的参数。
          //string sql2 = String.Format("select * from t_user where id='{0}' and psw='{1}'", textBox1.Text, textBox2.Text);
          //string sql3 = $"select * from t_user where id='{textBox1.Text}' and psw='{textBox2.Text}'";
          //if (sql == sql2 && sql2 == sql3)
          //{
          //    MessageBox.Show("1");
          //}

          IDataReader dc = dao.read(sql);//提取数据
          if (dc.Read())
          {
              MessageBox.Show("登录成功");
              return true;
          }
          else
          {
              MessageBox.Show("登录失败");
              return false;
          }
      }
      //管理员
      if (radioButtonAdmin.Checked == true)
      {
          Dao dao = new Dao();
          string sql= "select * from t_admin where id='" + textBox1.Text + "' and psw='" + textBox2.Text + "'";
          IDataReader dc = dao.read(sql);//提取数据
          if (dc.Read())
          {
              MessageBox.Show("登录成功");
              return true;
          }
          else
          {
              MessageBox.Show("登录失败");
              return false;
          }
      }
      return true;
  }
```

- ![[Pasted image 20260816080615.png|262]]
- ![[Pasted image 20260816080829.png|275]]

# 3-管理员相关页面的布局设计



# 4-图书显示和图书添加功能的实现



# 5-删除和修改功能的实现



# 6-查询功能的实现



# 7-用户对应页面的设计



# 8-用户借书还书功能的实现



# 附加-多行删除



# 改进
- 2-登陆功能的实现
	- sql注入有风险，换种方式。



# 小tips
- 及时清理解决方案。
![[Pasted image 20260816080529.png|259]]
- 