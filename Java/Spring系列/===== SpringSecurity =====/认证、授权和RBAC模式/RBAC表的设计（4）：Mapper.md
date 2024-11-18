---
title: RBAC表的设计（4）：Mapper
updated: 2021-08-10T06:39:02
created: 2021-08-10T06:36:55
---

RBAC表的设计（4）：Mapper
2021年8月10日
6:36

@Mapper
public interface SysUserMapper extends BaseMapper\<SysUser\> {
}

@Mapper
public interface SysRelationMapper extends BaseMapper\<SysRelation\> {
}

@Mapper
public interface SysRoleMapper extends BaseMapper\<SysRole\> {
@Select("select " +
" r.id, r.rolename, r.roleinfo, r.gmt_created, r.gmt_modified " +
"from " +
" sys_role r, sys_relation rl " +
"where " +
" rl.userid = \#{userid} and " +
" rl.roleid = r.id;")
List\<SysRole\> selectRolesByUserid(@Param("userid")Integer userid);

@Select("select " +
" r.id, r.rolename, r.roleinfo, r.gmt_created, r.gmt_modified " +
"from " +
" sys_role r, sys_user u, sys_relation rl " +
"where " +
" u.username = \#{username} and " +
" u.id = rl.userid and " +
" rl.roleid = r.id;")
List\<SysRole\> selectRolesByUsername(@Param("username")String username);
}
