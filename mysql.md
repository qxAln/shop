mysql忘记密码步骤

1.进入到*==C:\Program Files\MySQL\MySQL Server 8.0\bin>==*路径中输入语句：net start mysql

2.启动成功后输入：mysql -u root –p

​                                 密码为root（如果忘记密码直接回车）

3.然后依次输入语句

①use mysql；                      ②update` `user` `set` `authentication_string=``password``(“密码“) where user=”root”；

③flush ``privileges``；

④quit

4.在重新进入：mysql -u root –p