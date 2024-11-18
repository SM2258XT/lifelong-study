---
title: RBAC表的设计（6）：UserDetailsService
updated: 2021-08-12T16:54:08
created: 2021-08-09T17:12:58
---

@Service
public class CustomerUserDetailService implements **UserDetailsService** {
@Autowired
SysUserMapper sysUserMapper;
@Autowired
SysRoleMapper sysRoleMapper;
@Override
public **UserDetails** loadUserByUsername(String username) throws UsernameNotFoundException {
if (username == null)
throw new UsernameNotFoundException("用户名为空！");

SysUser user = sysUserMapper.selectOne(new QueryWrapper\<SysUser\>().eq("username", username));
if (user == null)
throw new UsernameNotFoundException("不存在用户【" + username + "】");

List\<SysRole\> roles = sysRoleMapper.selectRolesByUserid(user.getId());
if (roles == null)
throw new UsernameNotFoundException("用户【" + username + "】没有任何角色！");

List\<GrantedAuthority\> authorities = new ArrayList\<\>();
for (SysRole role : roles) {
authorities.add(new SimpleGrantedAuthority("ROLE\_" + role.getRolename()));
}
user.setAuthorities(authorities);
//把用户的密码取出来进行加密，然后再保存。
PasswordEncoder encoder = new BCryptPasswordEncoder();
String encodedPassword = encoder.encode(user.getPassword());
user.setPassword(encodedPassword);
return user; //**这个返回的user类型就是实体类SysUser，因为它实现了 UserDetails 接口！**
}
}
