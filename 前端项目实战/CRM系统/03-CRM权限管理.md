# 03-CRM权限管理

![](../../markdown_img/Pasted%20image%2020230119023307.png)

## 权限管RBAC基本概念

`RBAC` 是基于角色的访问控制( `Role-Based Access Control` )在 `RBAC` 中，权限与角色相关联，用户通过扮演适当的角色从而得到这些角色的权限。这样管理都是层级相互依赖的，权限赋予给角色，角色又赋予用户，这样的权限设计很清楚，管理起来很方便。

RBAC授权实际上是 `Who`、`What` 、`How` 三元组之间的关系，也就是 `Who` 对 `What` 进行 `How` 的操作，简单说明就是谁对什么资源做了怎样的操作。

## RBAC表结构设计

### 实体对应关系

用户 - 角色 - 资源实体间的对于关系

![](../../markdown_img/Pasted%20image%2020230119144054.png)

这里用户与角色实体对应关系为多对多，角色与资源对应关系同样为多对多关系，所以在实体设计上用户与角色间增加用户角色实体，将多对多的对应关系拆分为-对多，同理，角色与资源多对多对应关系拆分出中间实体对象权限实体。

### 表结构设计

从上面实体对应关系分析,权限表设计分为以下基本的五张表结构：用户表(t_user). 角色表(t_role).t_user, .role(用户角色表)、资源表(t_module)、权限表(t_tpermission),表结构关系如下:

![](../../markdown_img/Pasted%20image%2020230117154830.png)


|t_user|用户表||||
|:--:|:--:|:--:|:--:|:--|
||字段|字段类型|字段限制|字段描述|
|主键|id|int(11)|自增|id主键|
||user_name|varchar(20)|非空|用户名|
||user_pwd|varchar(100)|非空|用户密码|
||true_name|varchar(20)||真实姓名|
||email|varchar(30)||邮箱|
||phone|varchar(20)||电话|
||is_valid|int(4)||有效状态|
||create_date|datetime||创建时间|
||update_date|datetime||更新时间|

|t_role|角色表||||
|:--:|:--:|:--:|:--:|:--|
||字段|字段类型|字段限制|字段描述|
|主键|id|int(11)|自增|id主键|
||role_name|varchar(20)|非空|角色名|
||role_remarker|varchar(100)||角色备注|
||is_valid|int(4)||有效状态|
||create_date|datetime||创建时间|
||update_date|datetime||更新时间|


|t_user_role|用户角色表||||
|:--:|:--:|:--:|:--:|:--|
||字段|字段类型|字段限制|字段描述|
|主键|id|int(11)|自增|id主键|
||user_id|int(11)|用户id|用户id|
||role_id|int(11)|角色id|角色id|
||create_date|datetime||创建时间|
||update_date|datetime||更新时间|

|t_module|资源表||||
|:--:|:--:|:--:|:--:|:--|
||字段|字段类型|字段限制|字段描述|
|主键|id|int(11)|自增|id主键|
||module_name|varchar(20)||资源名|
||module_style|varchar(100)||资源样式|
||url|varchar(20)||资源url地址|
||parent_id|int(11)|非空|上级资源id|
||parent_opt_value|varchar(20)|非空|上级资源权限码|
||grade|int(11)|非空|层级|
||opt_value|varchar(30)||权限码|
||orders|int(11)|非空|排列号|
||isvalid|int(4)||有效状态|
||create_date|datetime||创建时间|
||update_date|datetime||更新时间|

|t_permission|权限表||||
|:--:|:--:|:--:|:--:|:--|
||字段|字段类型|字段限制|字段描述|
|主键|id|int(11)|自增|id主键|
||role_id|int(11)|角色id|角色id|
||module_id|int(11)|资源id|资源id|
||acl_value|varchar(20)|非空|权限码|
||create_date|datetime||创建时间|
||update_date|datetime||更新时间|

实现三大模块

**用户管理**

- 用户基本信息维护（t_user）
- 角色分配（t_user_role）

**角色管理**

- 角色基本信息维护（t_role）
- 角色授权与认证（t_permisssion）

**资源管理**

- 资源菜单信维护（t_module）


## 用户管理功能

### 用户数据查询

`sql`

```xml
<!-- 多条件查询 -->  
<select id="selectByParams" parameterType="com.xxxx.crm.query.UserQuery" resultType="com.xxxx.crm.vo.User">  
	select      
		<include refid="Base_Column_List"></include>  
	from      
		t_user  
	<where>  
		is_valid = 1    <!-- 用户名查询 -->  
		<if test="null != userName and userName != ''">  
			and user_name like concat('%',#{userName},'%')    
		</if>  
		<!-- 邮箱查询 -->  
		<if test="null != email and email != ''">  
			and email like concat('%',#{email},'%')    
		</if>  
		<!-- 手机号查询 -->  
		<if test="null != phone and phone != ''">  
			and phone like concat('%',#{phone},'%')    
		</if>  
	</where>  
</select>
```


`BaseService`

```java

/**  
 * 多条件查询  
 *  
 * @param baseQuery  
 * @return java.util.List<T>  
 */  
public List<T> selectByParams(BaseQuery baseQuery) throws DataAccessException{  
    return baseMapper.selectByParams(baseQuery);  
}

/**  
 * 查询数据表格对应的数据  
 *  
 * @param baseQuery  
 * @return java.util.Map<java.lang.String,java.lang.Object>  
 */  
public Map<String, Object> queryByParamsForTable(BaseQuery baseQuery) {  
    Map<String,Object> result = new HashMap<String,Object>();  
    PageHelper.startPage(baseQuery.getPage(),baseQuery.getLimit());  
    PageInfo<T> pageInfo =new PageInfo<T>(selectByParams(baseQuery));  
    result.put("count",pageInfo.getTotal());  
    result.put("data",pageInfo.getList());  
    result.put("code",0);  
    result.put("msg","");  
    return result;  
}
```

`UserController`

```java
/**  
 * 分页多条件查询用户列表  
 *  
 * @param userQuery  
 * @return java.util.Map<java.lang.String,java.lang.Object>  
 */  
@RequestMapping("list")  
@ResponseBody  
public Map<String,Object> selectByParams(UserQuery userQuery) {  
    return userService.queryByParamsForTable(userQuery);  
}
```

### 添加用户信息

**此时还不涉及到多用户进行角色的绑定**

`sql`

```xml
<!-- 多条件查询 -->  
<select id="selectByParams" parameterType="com.xxxx.crm.query.UserQuery" resultType="com.xxxx.crm.vo.User">  
	select      
		<include refid="Base_Column_List"></include>  
	from      
		t_user  
	<where>  
		is_valid = 1    <!-- 用户名查询 -->  
		<if test="null != userName and userName != ''">  
			and user_name like concat('%',#{userName},'%')    
		</if>  
		<!-- 邮箱查询 -->  
		<if test="null != email and email != ''">  
			and email like concat('%',#{email},'%')    
		</if>  
		<!-- 手机号查询 -->  
		<if test="null != phone and phone != ''">  
			and phone like concat('%',#{phone},'%')    
		</if>  
	</where>  
</select>
```

**service** 层逻辑

添加用户  
1. 参数校验  
	用户名userName     非空，唯一性  
	邮箱email          非空  
	手机号phone        非空，格式正确  
2. 设置参数的默认值  
	isValid           1  
	createDate        系统当前时间  
	updateDate        系统当前时间  
	默认密码            123456 -> md5加密  
3. 执行添加操作，判断受影响的行数

添加用户时需要进行用户与角色的关联

`BaseService`

```java
/**  
 * 添加用户  
 * @param user 用户信息  
 * @return void 无返回值  
 */  
@Transactional(propagation = Propagation.REQUIRED)  
public void addUser(User user) {  
    /* 1. 参数校验 */    checkUserParams(user.getUserName(), user.getEmail(), user.getPhone(), null);  
  
    /* 2. 设置参数的默认值 */    user.setIsValid(1);  
    user.setCreateDate(new Date());  
    user.setUpdateDate(new Date());  
    // 设置默认密码  
    user.setUserPwd(Md5Util.encode("123456"));  
  
    /* 3. 执行添加操作，判断受影响的行数 */
    AssertUtil.isTrue(userMapper.insertSelective(user) < 1, "用户添加失败！");   
}

/**  
 *  参数校验  
 *        用户名userName     非空，唯一性  
 *        邮箱email          非空  
 *        手机号phone        非空，格式正确  
 *  
 * @param userName  
 * @param email  
 * @param phone  
 * @return void  
 */
private void checkUserParams(String userName, String email, String phone, Integer userId) {  
    // 判断用户名是否为空  
    AssertUtil.isTrue(StringUtils.isBlank(userName), "用户名不能为空！");  
    // 判断用户名的唯一性  
    // 通过用户名查询用户对象  
    User temp = userMapper.queryUserByName(userName);  
    // 如果用户对象为空，则表示用户名可用；如果用户对象不为空，则表示用户名不可用   
    AssertUtil.isTrue(null != temp, "用户名已存在，请重新输入！");  
  
    // 邮箱 非空  
    AssertUtil.isTrue(StringUtils.isBlank(email), "用户邮箱不能为空！");  
  
    // 手机号 非空  
    AssertUtil.isTrue(StringUtils.isBlank(phone), "用户手机号不能为空！");  
  
    // 手机号 格式判断  
    AssertUtil.isTrue(!PhoneUtil.isMobile(phone), "手机号格式不正确！");  
}
```

`UserController`

```java
/**  
 * 添加用户  
 *  
 * @param user  
 * @return com.xxxx.crm.base.ResultInfo  
 */
@PostMapping("add")  
@ResponseBody  
public ResultInfo addUser(User user) {  
    userService.addUser(user);  
    return success("用户添加成功！");  
}
```

### 更新用户信息

`service`层

1. 参数校验  
	判断用户ID是否为空，且数据存在  
	用户名userName     非空，唯一性  
	邮箱email          非空  
	手机号phone        非空，格式正确  
2. 设置参数的默认值  
	updateDate        系统当前时间  
3. 执行更新操作，判断受影响的行数  


```java
/**  
 * 更新用户  
 *  
 * @param user  
 * @return void  
 */@Transactional(propagation = Propagation.REQUIRED)  
public void updateUser(User user) {  
    // 判断用户ID是否为空，且数据存在  
    AssertUtil.isTrue(null == user.getId(), "待更新记录不存在！");  
    // 通过id查询数据  
    User temp = userMapper.selectByPrimaryKey(user.getId());  
    // 判断是否存在  
    AssertUtil.isTrue(null == temp, "待更新记录不存在！");  
    // 参数校验  
    checkUserParams(user.getUserName(), user.getEmail(),user.getPhone(), user.getId());  
  
    // 设置默认值  
    user.setUpdateDate(new Date());  
  
    // 执行更新操作，判断受影响的行数  
    AssertUtil.isTrue(userMapper.updateByPrimaryKeySelective(user) != 1, "用户更新失败！");  
}
```

这种情况下，可以更新成功，但是此时如果不更新`userName`的值为新的值，那么会导致用户名已存在的报错，此时就需要在验证详细中更改对`userName`的验证

```java
// 如果用户名存在，且与当前修改记录不是同一个，则表示其他记录占用了该用户名，不可用  
AssertUtil.isTrue(null != temp && !(temp.getId().equals(userId)), "用户名已存在，请重新输入！");
```

`userController`

```java
/**  
 * 更新用户  
 *  
 * @param user  
 * @return com.xxxx.crm.base.ResultInfo  
 */
@PostMapping("update")  
@ResponseBody  
public ResultInfo updateUser(User user) {  
    userService.updateUser(user);  
    return success("用户更新成功！");  
}
```

### 删除用户信息

`userService`

```java
/**  
 * 用户删除  
 * @param ids  
 * @return void  
 */
@Transactional(propagation = Propagation.REQUIRED)  
public void deleteByIds(Integer[] ids) {  
    // 判断ids是否为空，长度是否大于0  
    AssertUtil.isTrue(ids == null || ids.length == 0, "待删除记录不存在！");  
    // 执行删除操作，判断受影响的行数  
    AssertUtil.isTrue(userMapper.deleteBatch(ids) != ids.length, "用户删除失败！");  
  
    // 遍历用户ID的数组  
    for (Integer userId : ids) {  
        // 通过用户ID查询对应的用户角色记录  
        Integer count  = userRoleMapper.countUserRoleByUserId(userId);  
        // 判断用户角色记录是否存在  
        if (count > 0) {  
           //  通过用户ID删除对应的用户角色记录  
            AssertUtil.isTrue(userRoleMapper.deleteUserRoleByUserId(userId) != count, "删除用户失败！");  
        }  
    }  
}
```

`userController`

```java
/**  
 * 用户删除  
 *  
 * @param ids  
 * @return com.xxxx.crm.base.ResultInfo  
 */
@DeleteMapping("delete")  
@ResponseBody  
public ResultInfo deleteUser(Integer[] ids) {  
  
    userService.deleteByIds(ids);  
  
    return success("用户删除成功！");  
}
```

### 用户角色关联

在对用户进行操作同时更改对应的数据

在`service`层新增用户角色关联方法

逻辑：

- 添加操作  
	- 原始角色不存在  
		1. 不添加新的角色记录    不操作用户角色表  
		2. 添加新的角色记录      给指定用户绑定相关的角色记录  

- 更新操作  
	- 原始角色不存在  
		1. 不添加新的角色记录     不操作用户角色表  
		2. 添加新的角色记录       给指定用户绑定相关的角色记录  
	- 原始角色存在  
		1. 添加新的角色记录       判断已有的角色记录不添加，添加没有的角色记录  
		2. 清空所有的角色记录     删除用户绑定角色记录  
		3. 移除部分角色记录       删除不存在的角色记录，存在的角色记录保留  
		4. 移除部分角色，添加新的角色    删除不存在的角色记录，存在的角色记录保留，添加新的角色 

如何进行角色分配？？？  

**判断用户对应的角色记录存在，先将用户原有的角色记录删除，再添加新的角色记录（比较简单，但是这样会导致创建时间与更新时间永远一致）**  

- 删除操作  
	- 删除指定用户绑定的角色记录  


```java
/**  
 * 用户角色关联  
 *  
 * @param userId  用户ID  
 * @param roleIds 角色ID  
 * @return  
 */  
private void relationUserRole(Integer userId, String roleIds) {  
  
    // 通过用户ID查询角色记录  
    Integer count = userRoleMapper.countUserRoleByUserId(userId);  
    // 判断角色记录是否存在  
    if (count > 0) {  
        // 如果角色记录存在，则删除该用户对应的角色记录  
        AssertUtil.isTrue(userRoleMapper.deleteUserRoleByUserId(userId) != count, "用户角色分配失败！");  
    }  
  
    // 判断角色ID是否存在，如果存在，则添加该用户对应的角色记录  
    if (StringUtils.isNotBlank(roleIds)) {  
        // 将用户角色数据设置到集合中，执行批量添加  
        List<UserRole> userRoleList = new ArrayList<>();  
        // 将角色ID字符串转换成数组  
        String[] roleIdsArray = roleIds.split(",");  
        // 遍历数组，得到对应的用户角色对象，并设置到集合中  
        for (String roleId : roleIdsArray) {  
            UserRole userRole = new UserRole();  
            userRole.setRoleId(Integer.parseInt(roleId));  
            userRole.setUserId(userId);  
            userRole.setCreateDate(new Date());  
            userRole.setUpdateDate(new Date());  
            // 设置到集合中  
            userRoleList.add(userRole);  
        }  
        // 批量添加用户角色记录  
        AssertUtil.isTrue(userRoleMapper.insertBatch(userRoleList) != userRoleList.size(), "用户角色分配失败！");  
    }  
  
}
```

在添加、更新操作中进行用户角色关联

```java
/* 用户角色关联 */
/**  
 * 用户ID  
 *  userId 
 * 角色ID  
 *  roleIds 
 */
relationUserRole(user.getId(), user.getRoleIds());
```

设置添加数据返回主键（需要获取主键来绑定，所以需要在执行完操作后，返回userId）

```xml
<!--  
   添加操作  
      默认返回的饿是受影响的行数，可以设置返回主键（自动增长）  
      useGeneratedKeys:取值范围是true或false，表示会获取主键，并赋值到keyProperty属性设置的模型属性（JavaBean实体类中的属性字段）  
      keyProperty:设置返回值将赋值给数据属性的哪个属性字段  
      keyColumn:设置数据库自动生动的朱主键名  
  
      返回的主键会自动设置到实体类中对应的id属性字段中  
-->  
<insert id="insertSelective" parameterType="com.xxxx.crm.vo.User" useGeneratedKeys="true" keyProperty="id" keyColumn="id" >  
  insert into t_user  <trim prefix="(" suffix=")" suffixOverrides="," >  
    <if test="id != null" >  
      id,    </if>  
    <if test="userName != null" >  
      user_name,    </if>  
    <if test="userPwd != null" >  
      user_pwd,    </if>  
    <if test="trueName != null" >  
      true_name,    </if>  
    <if test="email != null" >  
      email,    </if>  
    <if test="phone != null" >  
      phone,    </if>  
    <if test="isValid != null" >  
      is_valid,    </if>  
    <if test="createDate != null" >  
      create_date,    </if>  
    <if test="updateDate != null" >  
      update_date,    </if>  
  </trim>  
  <trim prefix="values (" suffix=")" suffixOverrides="," >  
    <if test="id != null" >  
      #{id,jdbcType=INTEGER},    </if>  
    <if test="userName != null" >  
      #{userName,jdbcType=VARCHAR},    </if>  
    <if test="userPwd != null" >  
      #{userPwd,jdbcType=VARCHAR},    </if>  
    <if test="trueName != null" >  
      #{trueName,jdbcType=VARCHAR},    </if>  
    <if test="email != null" >  
      #{email,jdbcType=VARCHAR},    </if>  
    <if test="phone != null" >  
      #{phone,jdbcType=VARCHAR},    </if>  
    <if test="isValid != null" >  
      #{isValid,jdbcType=INTEGER},    </if>  
    <if test="createDate != null" >  
      #{createDate,jdbcType=TIMESTAMP},    </if>  
    <if test="updateDate != null" >  
      #{updateDate,jdbcType=TIMESTAMP},    </if>  
  </trim>  
</insert>
```

当删除用户时，就将此用户相关的所有的用户角色关联的数据删除

## 角色管理功能实现

### 角色查询

对所有的角色进行查询

`sql`

```xml
<!-- 多条件查询 -->  
<select id="selectByParams" parameterType="com.xxxx.crm.query.RoleQuery" resultType="com.xxxx.crm.vo.Role">  
	select      
		<include refid="Base_Column_List"></include>  
	from      
		t_role  
	<where>  
		is_valid = 1    
		<if test="null != roleName and '' != roleName">  
			and role_name like concat('%',#{roleName},'%')    
		</if>  
	</where>  
</select>
```


`roleController`

```java
/**  
 * 分页条件查询角色列表  
 *  
 * @param roleQuery  
 * @return java.util.Map<java.lang.String,java.lang.Object>  
 */  
@GetMapping("list")  
@ResponseBody  
public Map<String,Object> selectByParams(RoleQuery roleQuery) {  
    return roleService.queryByParamsForTable(roleQuery);  
}
```

### 添加角色

`sql`

```xml
<!-- 通过角色名查询角色记录 -->  
<select id="selectByRoleName" parameterType="string" resultType="com.xxxx.crm.vo.Role">  
	select      
		<include refid="Base_Column_List"></include>  
	from      
		t_role  
	where      
		is_valid = 1  
	and      
		role_name = #{roleName}
</select>
```

`roleMapper`

```java
// 通过角色名查询角色记录  
Role selectByRoleName(String roleName);
```

`roleService`

1. 参数校验  
	角色名称        非空，名称唯一  
2. 设置参数的默认值  
	是否有效  
	创建时间  
	修改时间  
3. 执行添加操作，判断受影响的行数  

```java
/**  
 * 添加角色  
 *  
 * @param role  
 * @return void  
 */
@Transactional(propagation = Propagation.REQUIRED)  
public void addRole(Role role) {  
    /* 1. 参数校验 */    
    AssertUtil.isTrue(StringUtils.isBlank(role.getRoleName()), "角色名称不能为空！");  
    // 通过角色名称查询角色记录  
    Role temp = roleMapper.selectByRoleName(role.getRoleName());  
    // 判断角色记录是否存在（添加操作时，如果角色记录存在则表示名称不可用）  
    AssertUtil.isTrue(temp != null, "角色名称已存在，请重新输入！");  
  
    /* 2. 设置参数的默认值  */    
    // 是否有效  
    role.setIsValid(1);  
    // 创建时间  
    role.setCreateDate(new Date());  
    // 修改时间  
    role.setUpdateDate(new Date());  
  
	/* 3. 执行添加操作，判断受影响的行数 */
	AssertUtil.isTrue(roleMapper.insertSelective(role) < 1, "角色添加失败！");  
}
```

`roleControler`

```java
/**  
 * 添加角色  
 *  
 * @param role  
 * @return com.xxxx.crm.base.ResultInfo  
 */
@PostMapping("add")  
@ResponseBody  
public ResultInfo addRole(Role role) {  
    roleService.addRole(role);  
    return success("角色添加成功！");  
}
```

### 更新角色

`roleService`

1. 参数校验  
	角色ID    非空，且数据存在  
	角色名称   非空，名称唯一  
2. 设置参数的默认值  
	修改时间  
3. 执行更新操作，判断受影响的行数

```java
/**  
 * 修改角色  
 * 
 * @param role  
 * @return void  
 */
@Transactional(propagation = Propagation.REQUIRED)  
public void updateRole(Role role) {  
    /* 1. 参数校验 */    
    // 角色ID    非空，且数据存在  
    AssertUtil.isTrue(null == role.getId(), "待更新记录不存在！");  
    // 通过角色ID查询角色记录  
    Role temp = roleMapper.selectByPrimaryKey(role.getId());  
    // 判断角色记录是否存在  
    AssertUtil.isTrue(null == temp, "待更新记录不存在");  
  
    // 角色名称   非空，名称唯一  
    AssertUtil.isTrue(StringUtils.isBlank(role.getRoleName()), "角色名称不能为空！");  
    // 通过角色名称查询角色记录  
    temp = roleMapper.selectByRoleName(role.getRoleName());  
    // 判断角色记录是否存在（如果不存在，表示可使用；如果存在，且角色ID与当前更新的角色ID不一致，表示角色名称不可用）  
    AssertUtil.isTrue(null != temp && (!temp.getId().equals(role.getId())), "角色名称已存在，不可使用！");  
  
    /* 2. 设置参数的默认值 */    
    role.setUpdateDate(new Date());  
  
    /* 3. 执行更新操作，判断受影响的行数 */
    AssertUtil.isTrue(roleMapper.updateByPrimaryKeySelective(role) < 1, "修改角色失败！");  
}
```

`roleController`

```java
/**  
 * 修改角色  
 *  
 * @param role  
 * @return com.xxxx.crm.base.ResultInfo  
 */
@PostMapping("update")  
@ResponseBody  
public ResultInfo updateRole(Role role) {  
    roleService.updateRole(role);  
    return success("角色修改成功！");  
}
```

### 删除角色

`service`层

`roleService`

 1. 参数校验  
     角色ID    非空，数据存在  
 2. 设置相关参数的默认  
     是否有效    0（删除记录）  
     修改时间    系统默认时间  
 3. 执行更新操作，判断受影响的行数  

```java
 /** 删除角色  
 *  
 * @param roleId  
 * @return void  
 */
@Transactional(propagation = Propagation.REQUIRED)  
public void deleteRole(Integer roleId) {  
    // 判断角色ID是否为空  
    AssertUtil.isTrue(null == roleId, "待删除记录不存在！");  
    // 通过角色ID查询角色记录  
    Role role = roleMapper.selectByPrimaryKey(roleId);  
    // 判断角色记录是否存在  
    AssertUtil.isTrue(null == role, "待删除记录不存在！");  
  
    if (role.getIsValid() == 0) {  
        AssertUtil.isTrue(true, "该角色已经删除，不需要重复删除！");  
    } else {  
        // 设置删除状态  
        role.setIsValid(0);  
        role.setUpdateDate(new Date());  
  
        // 执行更新操作  
        AssertUtil.isTrue(roleMapper.updateByPrimaryKeySelective(role) < 1, "角色删除失败！");  
    }  
}
```

`roleController`

```java
/**  
 * 删除角色  
 *  
 * @param roleId  
 * @return com.xxxx.crm.base.ResultInfo  
 */
@PostMapping("delete")  
@ResponseBody  
public ResultInfo deleteRole(Integer roleId) {  
    roleService.deleteRole(roleId);  
    return success("角色删除成功！");  
}
```

### 角色资源授权

`service`层

`roleService`

将对应的角色ID与资源ID，添加到对应的权限表中 

直接添加权限：不合适，会出现重复的权限数据（执行修改权限操作后删除权限操作时）  

推荐使用：先将已有的权限记录删除，再将需要设置的权限记录添加  

1. 通过角色ID查询对应的权限记录  
2. 如果权限记录存在，则删除对应的角色拥有的权限记录  
3. 如果有权限记录，则添加权限记录 (批量添加) 

```java
/**  
 * 角色授权  
 *   
 * @param roleId  
 * @param mIds  
 * @return void  
 */
@Transactional(propagation = Propagation.REQUIRED)  
public void addGrant(Integer roleId, Integer[] mIds) {
	// 检验参数  
	AssertUtil.isTrue(null == roleId, "待授权记录不存在！");  
	AssertUtil.isTrue(null == mIds, "待授权资源不存在！");

    // 1. 通过角色ID查询对应的权限记录  
    Integer count = permissionMapper.countPermissionByRoleId(roleId);  
    // 2. 如果权限记录存在，则删除对应的角色拥有的权限记录  
    if (count > 0) {  
        // 删除权限记录  
        permissionMapper.deletePermissionByRoleId(roleId);  
    }  
    // 3. 如果有权限记录，则添加权限记录  
    if (mIds != null &&  mIds.length > 0) {  
        // 定义Permission集合  
        List<Permission> permissionList = new ArrayList<>();  
  
        // 遍历资源ID数组  
        for(Integer mId: mIds) {  
            Permission permission = new Permission();  
            permission.setModuleId(mId);  
            permission.setRoleId(roleId);             
            permission.setAclValue(
		        moduleMapper.selectByPrimaryKey(mId).getOptValue());  
            permission.setCreateDate(new Date());  
            permission.setUpdateDate(new Date());  
            // 将对象设置到集合中  
            permissionList.add(permission);  
        }  
  
        // 执行批量添加操作，判断受影响的行数  
        AssertUtil.isTrue(
	        permissionMapper.insertBatch(permissionList) != permissionList.size(),
	        "角色授权失败！");  
    }  
}
```

`permissionMapper`

```xml
<!-- 通过角色ID查询权限记录 -->  
<select id="countPermissionByRoleId" parameterType="int" resultType="java.lang.Integer">  
  select      count(1)  from      t_permission  where      role_id = #{roleId}</select>  
  
<!-- 通过角色ID删除权限记录 -->  
<delete id="deletePermissionByRoleId" parameterType="int">  
  delete from      t_permission  where      role_id = #{roleId}</delete>  
  
<!-- 批量添加 -->  
<insert id="insertBatch">  
  insert  into      t_permission (role_id,module_id,acl_value,create_date,update_date)  values      <foreach collection="list" item="item" separator=",">  
        (#{item.roleId},#{item.moduleId},#{item.aclValue},now(),now())      </foreach>  
</insert>
```

```java
// 通过角色ID查询权限记录  
Integer countPermissionByRoleId(Integer roleId);

// 通过角色ID删除权限记录  
void deletePermissionByRoleId(Integer roleId);
```

`roleController`

```java
/**  
 * 角色授权  
 *  
 * @param roleId  
 * @param mIds  
 * @return com.xxxx.crm.base.ResultInfo  
 */
@PostMapping("addGrant")  
@ResponseBody  
public ResultInfo addGrant(Integer roleId, Integer[] mIds) {  
  
    roleService.addGrant(roleId, mIds);  
  
    return success("角色授权成功！");  
}
```

### 角色权限认证

当完成角色权限添加功能后，下一 步就是对角色操作的资源进行认证操作，这里对于认证包含两块:

1. 菜单级别显示控制
2. 后端方法访问控制 

**菜单级别访问控制实现**

系统根据登录用户扮演的不同角色来对登录用户操作的菜单进行动态控制显示操作

`indexController`

```java
// 通过当前登录用户ID查询当前登录用户拥有的资源列表 （查询对应资源的授权码）  
List<String> permissions = permissionService.queryUserHasRoleHasPermissionByUserId(userId);
```

`PermissionService`

```java
/***  
 * 通过查询用户拥有的角色，角色拥有的资源，得到用户拥有的资源列表 （资源权限码）  
 *  
 * @param userId  
 * @return java.util.List<java.lang.String>  
 */  
public List<String> queryUserHasRoleHasPermissionByUserId(Integer userId) {  
    return permissionMapper.queryUserHasRoleHasPermissionByUserId(userId);  
}
```

`sql`

```xml
<!-- 通过用户ID查询对应的资源列表（资源权限码） -->  
<select id="queryUserHasRoleHasPermissionByUserId" parameterType="int" resultType="java.lang.String">  
	SELECT DISTINCT      
		acl_value  
	FROM      
		t_user_role ur      
	LEFT JOIN 
		t_permission p ON ur.role_id = p.role_id  
	WHERE      
		ur.user_id = #{userId}
</select>
```

**后端方法访问控制**

在方法上添加**自定义注解**，在调用方法时，进行权限的验证

例如：获取营销管理方法的授权

```java
/**  
 * 删除营销机会 101003  
 * * @param ids  
 * @return com.xxxx.crm.base.ResultInfo  
 */
@RequiredPermission(code = "101003")  
@DeleteMapping("delete")  
@ResponseBody  
public ResultInfo deleteSaleChance(Integer[] ids) {  
    // 调用Service层的删除方法  
    saleChanceService.deleteBatch(ids);  
    return success("营销机会数据删除成功！");  
}
```

在全局的异常处理中也是需要对切面中抛出的异常进行处理


## 资源管理

### 查询所有资源

**直接查询所有的**

`moduleController`

```java
/***  
 * 查询资源列表  
 *  
 * @param  
 * @return java.util.Map<java.lang.String,java.lang.Object>  
 */  
@RequestMapping("list")  
@ResponseBody  
public Map<String, Object>  queryModuleList() {  
    return moduleService.queryModuleList();  
}
```

`moduleService`

```java
/***  
 * 查询资源数据  
 *  
 * @param  
 * @return java.util.Map<java.lang.String,java.lang.Object>  
 */  
public Map<String,Object> queryModuleList() {  
    Map<String, Object> map = new HashMap<>();  
    // 查询资源列表  
    List<Module> moduleList = moduleMapper.queryModuleList();  
    map.put("code",0);  
    map.put("msg","");  
    map.put("count", moduleList.size());  
    map.put("data",moduleList);  
  
    return map;  
}
```

**查询当前用户拥有的资源列表**

只返回生成树形结构所需要的字段（便于在角色管理中修改修改角色资源授权，树形结构）

```json
{
	"id": 1,   
	"pId": -1,
	"name": "营销管理",
	"checked": false
}
```

`moduleService`

```java
/**  
 * 查询所有的资源列表  
 *  
 * @param  
 * @return java.util.List<com.xxxx.crm.model.TreeModel>  
 */  
public List<TreeModel> queryAllModules(Integer roleId) {  
	AssertUtil.isTrue(null == roleId,"角色id不能为空！");

    // 查询所有的资源列表  
    List<TreeModel> treeModelList = moduleMapper.queryAllModules();  
    // 查询指定角色已经授权过的资源列表 (查询角色拥有的资源ID)  
    List<Integer> permissionIds = permissionMapper.queryRoleHasModuleIdsByRoleId(roleId);  
    // 判断角色是否拥有资源ID  
    if (permissionIds != null && permissionIds.size() > 0) {  
        // 循环所有的资源列表，判断用户拥有的资源ID中是否有匹配的，如果有，则设置checked属性为true  
        treeModelList.forEach(treeModel -> {  
            // 判断角色拥有的资源ID中是否有当前遍历的资源ID  
            if (permissionIds.contains(treeModel.getId())) {  
                // 如果包含你，则说明角色授权过，设置checked为true  
                treeModel.setChecked(true);  
            }  
        });  
    }  
    return treeModelList;  
}
```

`moduleController`

```java
/**  
 * 查询所有的资源列表  
 *  
 * @param  
 * @return java.util.List<com.xxxx.crm.model.TreeModel>  
 */  
@RequestMapping("queryAllModules")  
@ResponseBody  
public List<TreeModel> queryAllModules(Integer roleId) {  
    return moduleService.queryAllModules(roleId);  
}
```

### 添加资源

`moculeService`

1. 参数校验  
    模块名称 moduleName  
        非空，同一层级下模块名称唯一  
    地址 url  
        二级菜单（grade=1），非空且同一层级下不可重复  
    父级菜单 parentId  
        一级菜单（目录 grade=0）    -1  
        二级|三级菜单（菜单|按钮 grade=1或2）    非空，父级菜单必须存在  
    层级 grade  
        非空，0|1|2  
    权限码 optValue  
        非空，不可重复  
2. 设置参数的默认值  
    是否有效 isValid    1  
    创建时间createDate  系统当前时间  
    修改时间updateDate  系统当前时间  
3. 执行添加操作，判断受影响的行数 

```java
/**  
 * 添加资源   
 * @param module  
 * @return void  
 */
@Transactional(propagation = Propagation.REQUIRED)  
public void addModule(Module module) {  
    /* 1. 参数校验  */    
    // 层级 grade 非空，0|1|2  
    Integer grade = module.getGrade();  
    AssertUtil.isTrue(null == grade || !(grade == 0 || grade == 1 || grade == 2),"菜单层级不合法！");  
  
    // 模块名称 moduleName  非空  
    AssertUtil.isTrue(StringUtils.isBlank(module.getModuleName()), "模块名称不能为空！");  
    // 模块名称 moduleName  同一层级下模块名称唯一  
    AssertUtil.isTrue(
	    null != moduleMapper.queryModuleByGradeAndModuleName(
		    grade, 
		    module.getModuleName()), 
		"改层级下模块名称已存在！");  
  
    // 如果是二级菜单 （grade=1)  
    if (grade == 1) {  
        // 地址 url   二级菜单（grade=1），非空  
        AssertUtil.isTrue(StringUtils.isBlank(module.getUrl()),"URL不能为空！");  
        // 地址 url   二级菜单（grade=1），且同一层级下不可重复  
        AssertUtil.isTrue(
	        null != moduleMapper.queryModuleByGradeAndUrl(grade,module.getUrl()),
	        "URL不可重复！");  
    }  
  
    // 父级菜单 parentId    一级菜单（目录 grade=0）    -1    
    if (grade == 0) {  
        module.setParentId(-1);  
    }  
    // 父级菜单 parentId    二级|三级菜单（菜单|按钮 grade=1或2）    非空，父级菜单必须存在  
    if (grade != 0) {  
        // 非空  
        AssertUtil.isTrue(null == module.getParentId(),"父级菜单不能为空！");  
        // 父级菜单必须存在 (将父级菜单的ID作为主键，查询资源记录)  
        AssertUtil.isTrue(
	        null == moduleMapper.selectByPrimaryKey(module.getParentId()), 
	        "请指定正确的父级菜单！");  
    }  
  
    // 权限码 optValue     非空  
    AssertUtil.isTrue(
	    StringUtils.isBlank(module.getOptValue()),
	    "权限码不能为空！");  
    // 权限码 optValue     不可重复  
    AssertUtil.isTrue(
	    null != moduleMapper.queryModuleByOptValue(module.getOptValue()),
	    "权限码已存在！");  
  
  
    /* 2. 设置参数的默认值  */    
    // 是否有效 isValid    1    
    module.setIsValid((byte) 1);  
    // 创建时间createDate  系统当前时间  
    module.setCreateDate(new Date());  
    // 修改时间updateDate  系统当前时间  
    module.setUpdateDate(new Date());  
  
    /* 3. 执行添加操作，判断受影响的行数 */
    AssertUtil.isTrue(moduleMapper.insertSelective(module) < 1, "添加资源失败！");  
  
}
```

`moduleMapper`

```java
// 通过层级与模块名查询资源对象  
Module queryModuleByGradeAndModuleName(@Param("grade") Integer grade, @Param("moduleName") String moduleName);

// 通过层级与URL查询资源对象  
Module queryModuleByGradeAndUrl(@Param("grade")Integer grade, @Param("url")String url);

/**  
 * 根据id 查询详情 (BaseMapper中)  
 *  
 * @param id  
 * @return  
 */  
T selectByPrimaryKey(ID id) throws DataAccessException;

// 通过权限码查询资源对象  
Module queryModuleByOptValue(String optValue);
```

```xml
<!-- 通过层级与模块名查询资源对象 -->  
<select id="queryModuleByGradeAndModuleName" resultType="com.xxxx.crm.vo.Module">  
	select      
		<include refid="Base_Column_List"></include>  
	from      
		t_module 
	where      
		is_valid = 1 
	and 
		grade = #{grade} 
	and 
		module_name = #{moduleName}
</select>  

<!-- 通过层级与URL查询资源对象 -->  
<select id="queryModuleByGradeAndUrl" resultType="com.xxxx.crm.vo.Module">  
	select      
		<include refid="Base_Column_List"></include>  
	from      
		t_module  
	where      
		is_valid = 1 
	and 
		grade = #{grade} 
	and 
		url = #{url}
</select>

<!-- 通过权限码查询资源对象 -->  
<select id="queryModuleByOptValue" resultType="com.xxxx.crm.vo.Module">  
	select      
		<include refid="Base_Column_List"></include>  
	from      
		t_module  
	where      
		is_valid = 1 
	and 
		opt_value = #{optValue}
</select>

<select id="selectByPrimaryKey" resultMap="BaseResultMap" parameterType="java.lang.Integer" >  
	select
		<include refid="Base_Column_List" />  
	from 
		t_module  
	where 
		id = #{id,jdbcType=INTEGER}
</select>
```

`moduleController`

```java
/**  
 * 添加资源  
 *  
 * @param module  
 * @return com.xxxx.crm.base.ResultInfo  
 */
@PostMapping("add")  
@ResponseBody  
public ResultInfo addModule(Module module) {  
  
    moduleService.addModule(module);  
    return success("添加资源成功！");  
}
```

### 修改资源

`moduleService`

1. 参数校验  
    id  
        非空，数据存在  
    层级 grade  
        非空 0|1|2  
    模块名称 moduleName  
        非空，同一层级下模块名称唯一 （不包含当前修改记录本身）  
    地址 url  
        二级菜单（grade=1），非空且同一层级下不可重复（不包含当前修改记录本身）  
    权限码 optValue  
        非空，不可重复（不包含当前修改记录本身）  
2. 设置参数的默认值  
     修改时间updateDate  系统当前时间  
3. 执行更新操作，判断受影响的行数  

```java
/**  
 * 修改资源  
 *  
 * @param module  
 * @return void  
 */
@Transactional(propagation = Propagation.REQUIRED)  
public void updateModule(Module module) {  
    /* 1. 参数校验 */    
    // id 非空，数据存在  
    // 非空判断  
    AssertUtil.isTrue(null == module.getId(), "待更新记录不存在！");  
    // 通过id查询资源对象  
    Module temp = moduleMapper.selectByPrimaryKey(module.getId());  
    // 判断记录是否存在  
    AssertUtil.isTrue(null == temp, "待更新记录不存在！");  
  
    // 层级 grade  非空 0|1|2    Integer grade = module.getGrade();  
    AssertUtil.isTrue(null == grade || !(grade == 0 || grade == 1 || grade == 2), "菜单层级不合法！");  
  
    // 模块名称 moduleName      非空，同一层级下模块名称唯一 （不包含当前修改记录本身）  
    AssertUtil.isTrue(StringUtils.isBlank(module.getModuleName()), "模块名称不能为空！");  
    // 通过层级与模块名称查询资源对象  
    temp = moduleMapper.queryModuleByGradeAndModuleName(grade, module.getModuleName());  
    if (temp != null) {  
        AssertUtil.isTrue(!(temp.getId()).equals(module.getId()), "该层级下菜单名已存在！");  
    }  
  
    // 地址 url   二级菜单（grade=1），非空且同一层级下不可重复（不包含当前修改记录本身）  
    if (grade == 1) {  
        AssertUtil.isTrue(StringUtils.isBlank(module.getUrl()), "菜单URL不能为空！");  
        // 通过层级与菜单URl查询资源对象  
        temp = moduleMapper.queryModuleByGradeAndUrl(grade, module.getUrl());  
        // 判断是否存在  
        if (temp != null) {  
            AssertUtil.isTrue(!(temp.getId()).equals(module.getId()), "该层级下菜单URL已存在！");  
        }  
    }  
  
    // 权限码 optValue     非空，不可重复（不包含当前修改记录本身）  
    AssertUtil.isTrue(StringUtils.isBlank(module.getOptValue()), "权限码不能为空！");  
    // 通过权限码查询资源对象  
    temp = moduleMapper.queryModuleByOptValue(module.getOptValue());  
    // 判断是否为空  
    if (temp != null) {  
        AssertUtil.isTrue(!(temp.getId()).equals(module.getId()),"权限码已存在！");  
    }  
  
    /* 2. 设置参数的默认值  */    // 修改时间 系统当前时间  
    module.setUpdateDate(new Date());  
  
    /* 3. 执行更新操作，判断受影响的行数 */
    AssertUtil.isTrue(moduleMapper.updateByPrimaryKeySelective(module) < 1, "修改资源失败！");  
}
```


`moduleController`

```java
/**  
 * 修改资源  
 *  
 * @param module  
 * @return com.xxxx.crm.base.ResultInfo  
 */
@PostMapping("update")  
@ResponseBody  
public ResultInfo updateModule(Module module) {  
  
    moduleService.updateModule(module);  
    return success("修改资源成功！");  
}
```

### 删除资源

`moduleService`

1. 判断删除的记录是否存在  
2. 如果当前资源存在子记录，则不可删除  
3. 删除资源时，将对应的权限表的记录也删除（判断权限表中是否存在关联数据，如果存在，则删除）  
4. 执行删除（更新）操作，判断受影响的行数  

```java
/**  
 * 删除资源  
 *  
 * @param id  
 * @return void  
 */
@Transactional(propagation = Propagation.REQUIRED)  
public void deleteModule(Integer id) {  
    // 判断id是否为空  
    AssertUtil.isTrue(null == id, "待删除记录不存在！");  
    // 通过id查询资源对象  
    Module temp = moduleMapper.selectByPrimaryKey(id);  
    // 判断资源对象是否为空  
    AssertUtil.isTrue(null == temp, "待删除记录不存在！");  
  
    // 如果当前资源存在子记录(将id当做父Id查询资源记录)  
    Integer count = moduleMapper.queryModuleByParentId(id);  
    // 如果存在子记录，则不可删除  
    AssertUtil.isTrue(count > 0, "该资源存在子记录，不可删除！");  
  
    // 通过资源id查询权限表中是否存在数据  
    count = permissionMapper.countPermissionByModuleId(id);  
    // 判断是否存在，存在则删除  
    if (count > 0) {  
        // 删除指定资源ID的权限记录  
        permissionMapper.deletePermissionByModuleId(id);  
    }  
  
    // 设置记录无效  
    temp.setIsValid((byte) 0);  
    temp.setUpdateDate(new Date());  
  
    // 执行更新  
    AssertUtil.isTrue(moduleMapper.updateByPrimaryKeySelective(temp) < 1, "删除资源失败！");  
}
```

`moduleController`

```java
/**  
 * 删除资源  
 * @param id  
 * @return com.xxxx.crm.base.ResultInfo  
 */
@PostMapping("delete")  
@ResponseBody  
public ResultInfo deleteModule(Integer id) {  
  
    moduleService.deleteModule(id);  
    return success("删除资源成功！");  
}
```

