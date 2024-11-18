---
title: MyUserDetailService
updated: 2021-08-09T15:45:20
created: 2021-08-08T16:41:33
---

@Service
public class MyUserDetailService implements UserDetailsService {
@Autowired
AccountMapper accountMapper;
@Override
public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
User user = null;
//用户名为空
if (username != null) {
QueryWrapper\<Account\> wrapper = new QueryWrapper\<\>();
wrapper.eq("username", username);
Account account = accountMapper.selectOne(wrapper);
//查到了该账户的记录
if (account != null) {
List\<GrantedAuthority\> authorities = new ArrayList\<\>();
//角色名必须以ROLE_开头！！
GrantedAuthority authority = new SimpleGrantedAuthority(**"ROLE\_"** + account.getRole());
authorities.add(authority);

String codedPassword = new BCryptPasswordEncoder().encode(account.getPassword());
user = new User(account.getUsername(), codedPassword, authorities);
}
}
return user;
}
}
注意：
1.  返回值UserDetails是一个接口，最简单的方式是返回其中之一实现类：User
2.  User的构造函数为：
User(String username, String password, Collection\<? extends GrantedAuthority\> authorities) ;

前两个参数通过数据库查出来

最后一个参数是集合，并且其中的元素要继承 GrantedAuthority，因此使用其子类SimpleGrantedAuthority()
1.  SimpleGrantedAuthority的构造函数为：
SimpleGrantedAuthority(String role);

这个role字符串必须加上前缀："ROLE\_"

在controller中使用的时候不需要写前缀。
1.  User构造函数中的密码，必须加密：
可以从数据库明文查到后在代码里加密（左侧），

也可以在存入数据库时直接加密存进去。
