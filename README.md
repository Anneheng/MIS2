##逻辑过程
###1、根据用户名查找对应的用户ID
###2、根据用户ID查找角色ID
###3、根据角色ID和用户ID查找权限，生成权限集
###4、根据权限集privilegeAccessKey查找对应的botton和menu
##SQL语句
create view test1permit as select * from cf_privilege 
where PrivilegeOperation='Permit' and ((PrivilegeMaster='CF_Role' and PrivilegeMasterKey=(select RoleID from cf_user,cf_userrole where cf_user.UserID=cf_userrole.UserID and LoginName='test1'))
or (PrivilegeMaster='CF_User' and PrivilegeMasterKey=(select UserID from cf_user where LoginName='test1') ));

select distinct MenuID,MenuNo,MenuName,MenuUrl from sys_menu,test1permit where
sys_menu.MenuID=test1permit.PrivilegeAccessKey

UNION

select distinct BtnID,BtnNo,BtnName,MENUNO from sys_button,test1permit where
sys_button.BtnID=test1permit.PrivilegeAccessKey;
