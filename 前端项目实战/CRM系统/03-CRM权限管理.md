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

## 用户角色关联

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