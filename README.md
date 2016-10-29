##逻辑过程
###1、根据用户名查找对应的用户ID
###2、根据用户ID查找角色ID
###3、根据角色ID和用户ID查找权限，生成权限集
###4、根据权限集privilegeAccessKey查找对应的botton和menu
##SQL语句
create view test1permit as select * from cf_privilege 
where PrivilegeOperation='Permit' and ((PrivilegeMaster='CF_Role' and PrivilegeMasterKey=(select RoleID from cf_user,cf_userrole where cf_user.UserID=cf_userrole.UserID and LoginName='test1'))
or (PrivilegeMaster='CF_User' and PrivilegeMasterKey=(select UserID from cf_user where LoginName='test1') ));

select distinct MenuNo as '编号',MenuName as '名称',MenuNo as '页面或订单' from sys_menu,test1permit where
sys_menu.MenuID=test1permit.PrivilegeAccessKey

UNION

select distinct BtnNo,BtnName,MenuNo from sys_button,test1permit where
sys_button.BtnID=test1permit.PrivilegeAccessKey;
##查询结果截图
![image](https://github.com/Anneheng/MIS2/blob/master/1.PNG)
![image](https://github.com/Anneheng/MIS2/blob/master/2.PNG)
