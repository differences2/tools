# UserDetailServiceImpl, Utils/UserDetailImpl

## 使用spring-boot-starter-security依赖后，如果使其对应到数据库

/backend/service/impl/

- UserDetailsServiceImpl:

```java
package org.eoa.backend.service.impl;

import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
import org.eoa.backend.mapper.UserMapper;
import org.eoa.backend.pojo.User;
import org.eoa.backend.service.impl.utils.UserDetailsImpl;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;

@Service
public class UserDetailsServiceImpl implements UserDetailsService {

    @Autowired
    private UserMapper userMapper;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {  // alt+insert overwrite

        QueryWrapper<User> wrapper = new QueryWrapper<>();
        wrapper.eq("username", username);
        User user = userMapper.selectOne(wrapper);
        if (user == null) {
            throw new RuntimeException("用户不存在");
        }
        return new UserDetailsImpl(user);
    }
}
```

/backend/service/impl/utils/

- UserDetailsImpl:

```java
package org.eoa.backend.service.impl.utils;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import org.eoa.backend.pojo.User;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails;

import java.util.Collection;
import java.util.Collections;

@Data
@NoArgsConstructor
@AllArgsConstructor
public class UserDetailsImpl implements UserDetails {

    private User user;

    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {  // alt+insert implement
        return Collections.emptyList();
    }

    @Override
    public String getPassword() {
        return user.getPassword();
    }

    @Override
    public String getUsername() {
        return user.getUsername();
    }

    @Override
    public boolean isAccountNonExpired() {
        return true;
    }

    @Override
    public boolean isAccountNonLocked() {
        return true;
    }

    @Override
    public boolean isCredentialsNonExpired() {
        return true;
    }

    @Override
    public boolean isEnabled() {
        return true;
    }
}
```