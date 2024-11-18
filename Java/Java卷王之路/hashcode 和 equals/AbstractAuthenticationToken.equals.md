---
title: AbstractAuthenticationToken.equals
updated: 2022-10-25T13:56:24
created: 2022-07-12T13:41:22
---

AbstractAuthenticationToken**重写了equals()和hashcode()**
public boolean equals(Object obj) {
if (!(obj instanceof AbstractAuthenticationToken)) {
return false;
} else {
AbstractAuthenticationToken test = (AbstractAuthenticationToken)obj;
if (!this.authorities.equals(test.authorities)) {
return false;
} else if (this.details == null && test.getDetails() != null) {
return false;
} else if (this.details != null && test.getDetails() == null) {
return false;
} else if (this.details != null && !this.details.equals(test.getDetails())) {
return false;
} else if (this.getCredentials() == null && test.getCredentials() != null) {
return false;
} else if (this.getCredentials() != null && !this.getCredentials().equals(test.getCredentials())) {
return false;
} else if (this.getPrincipal() == null && test.getPrincipal() != null) {
return false;
} else if (this.getPrincipal() != null && !this.getPrincipal().equals(test.getPrincipal())) {
return false;
} else {
return this.isAuthenticated() == test.isAuthenticated();
}
}
}

public int hashCode() {
int code = 31;

GrantedAuthority authority;
for(Iterator var2 = this.authorities.iterator(); var2.hasNext(); code ^= authority.hashCode()) {
authority = (GrantedAuthority)var2.next();
}

if (this.getPrincipal() != null) {
code ^= this.getPrincipal().hashCode();
}

if (this.getCredentials() != null) {
code ^= this.getCredentials().hashCode();
}

if (this.getDetails() != null) {
code ^= this.getDetails().hashCode();
}

if (this.isAuthenticated()) {
code ^= -37;
}

return code;
}
