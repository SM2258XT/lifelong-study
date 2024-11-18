---
title: RBAC表的设计（3）：实体类
updated: 2021-08-09T17:15:00
created: 2021-08-09T17:07:20
---

@Data
@AllArgsConstructor
@NoArgsConstructor
public class **SysRole** {
@TableId(type = IdType.AUTO)
private Integer id;
private String rolename;
private String roleinfo;

@TableField(fill = FieldFill.INSERT)
private Date gmtCreated;
@TableField(fill = FieldFill.INSERT_UPDATE)
private Date gmtModified;

public SysRole(String rolename, String roleinfo) {
this.rolename = rolename;
this.roleinfo = roleinfo;
}
}
这个实体类SysUser直接实现了UserDetails接口，可以被框架直接使用！

@Data
@AllArgsConstructor
@NoArgsConstructor
public class **SysUser implements UserDetails** {
@TableId(type = IdType.AUTO)
private Integer id;

private String username;
private String password;
private String realname;

private boolean isEnabled;
private boolean isCredentials;
private boolean isNonExpired;
private boolean isNonLocked;

@TableField(exist = false) //忽略权限字段
List\<GrantedAuthority\> authorities;

@TableField(fill = FieldFill.INSERT)
private Date gmtCreated;
@TableField(fill = FieldFill.INSERT_UPDATE)
private Date gmtModified;

public SysUser(String username, String password, String realname, boolean isEnabled, boolean isCredentials, boolean isNonExpired, boolean isNonLocked) {
this.username = username;
this.password = password;
this.realname = realname;
this.isEnabled = isEnabled;
this.isCredentials = isCredentials;
this.isNonExpired = isNonExpired;
this.isNonLocked = isNonLocked;
}

@Override
public Collection\<? extends GrantedAuthority\> getAuthorities() {
return this.authorities;
}

@Override
public String getPassword() {
return this.password;
}

@Override
public String getUsername() {
return this.username;
}

@Override
public boolean isAccountNonExpired() {
return this.isNonExpired;
}

@Override
public boolean isAccountNonLocked() {
return this.isNonLocked;
}

@Override
public boolean isCredentialsNonExpired() {
return this.isCredentials;
}

@Override
public boolean isEnabled() {
return this.isEnabled;
}
}
@Data
@AllArgsConstructor
@NoArgsConstructor
public class **SysRelation** {
@TableId(type = IdType.AUTO)
private Integer id;

private Integer userid;
private Integer roleid;

@TableField(fill = FieldFill.INSERT)
private Date gmtCreated;
@TableField(fill = FieldFill.INSERT_UPDATE)
private Date gmtModified;

public SysRelation(Integer userid, Integer roleid) {
this.userid = userid;
this.roleid = roleid;
}
}
