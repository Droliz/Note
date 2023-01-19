# 02-CRM营销管理

![](../../markdown_img/Pasted%20image%2020230117213346.png)

## 营销管理

### 营销管理表结构分析

针对企业中客户的质询需求所建立的信息录入功能，方便销售人员进行后续的客户需求跟踪与联系，提高企业客户购买产品的几率。

对于系统录入每条营销机会记录，系统会分配对应的销售人员与对应的客户进行详细了解，从而提高客户开发计划的成功几率。对应到营销机会管理模块功能上。即:营销机会数据的录入，分配，修改，删除与查询等基本操作。

![](../../markdown_img/Pasted%20image%2020230117153006.png)

表结构如下

|t_sale_chance|营销机会表||||
|:--:|:--:|:--:|:--:|:--|
||字段|字段类型|字段限制|字段描述|
|主键|id|int(11)|自增|id主键|
||chance_source|varchar(300)||机会来源|
||customer_name|varchar(100)||客户名称|
||cgjl|int(11)||成功几率|
||overview|varchar(300)||概要|
||link_man|varchar(100)||联系人|
||link_phone|varchar(100)||手机号|
||description|varchar(1000)||描述|
||create_man|varchar(100)||创建人|
||assign_man|varchar(100)||分配人|
||assign_time|datetime||分配时间|
||state|int(11)||分配状态|
||dev_result|int(11)||开发结果|
||is_valid|int(4)||有效状态(删除并不真正删除，仅仅更改此值)|
||create_date|datetime||创建时间|
||update_date|datetime||更新时间|

|t_cus_dev_plan|客户开发计划表||||
|:--:|:--:|:--:|:--:|:--|
||字段|字段类型|字段限制|字段描述|
|主键|id|int(11)|自增|id主键|
||sale_chance_id|int(11)||营销机会id|
||plan_item|varchar(100)||计划内容|
||plan_date|datetime||计划日期|
||exe_affect|varchar(100)||执行效果|
||is_valid|int(4)||有效状态|
||create_date|datetime||创建时间|
||update_date|datetime||更新时间|

### 营销机会数据查询

`saleChanceController`

```java
@RequestMapping("list")  
@ResponseBody  
public Map<String, Object> querySaleChanceByParams(SaleChanceQuery saleChanceQuery) {  
    return saleChanceService.querySaleChanceByParams(saleChanceQuery);  
}
```

`saleChanceService`

```java
/**  
 * 多条件分页查询营销机会
 *  
 * @param saleChanceQuery 查询参数  
 * @return java.util.Map<java.lang.String, java.lang.Object>  
 */  
public Map<String, Object> querySaleChanceByParams(SaleChanceQuery saleChanceQuery) {  
  
    Map<String, Object> map = new HashMap<>();  
  
    // 开启分页  
    PageHelper.startPage(saleChanceQuery.getPage(), saleChanceQuery.getLimit());  
    // 得到对应分页对象  (BaseMapper中有)
    PageInfo<SaleChance> pageInfo = new PageInfo<>(saleChanceMapper.selectByParams(saleChanceQuery));  
  
    // 设置map对象  
    map.put("code",0);  
    map.put("msg","success");  
    map.put("count",pageInfo.getTotal());  
    // 设置分页好的列表  
    map.put("data",pageInfo.getList());  
  
    return map;  
}
```

自定义查询

```xml
<!-- 多条件查询 -->  
<select id="selectByParams" parameterType="com.xxxx.crm.query.SaleChanceQuery" resultType="com.xxxx.crm.vo.SaleChance">  
  select  s.id, chance_source, customer_name, cgjl, overview, link_man, link_phone, description,  create_man, assign_man, assign_time, state, dev_result, s.is_valid, s.create_date, s.update_date  ,u.user_name as uname  from      t_sale_chance s  left join      t_user u  on      s.assign_man = u.id  
  <where>  
    s.is_valid = 1    
    <if test="customerName != null and customerName != ''">  
      and s.customer_name like concat('%',#{customerName},'%')    
      </if>  
    <if test="createMan != null and createMan != ''">  
      and s.create_man = #{createMan}    
      </if>  
    <if test="state != null">  
      and s.state = #{state}    
      </if>  
    <!-- 根据开发状态进行查询 -->  
    <if test="devResult != null and devResult != ''">  
      and s.dev_result = #{devResult}    
      </if>  
    <!-- 根据指派人进行查询 -->  
    <if test="assignMan != null">  
      and s.assign_man = #{assignMan}    
	  </if>  
  </where>  
</select>
```

### 添加营销机会数据

`SaleChanceService`

`service` 层逻辑

1. 参数校验  
	customerName客户名称    非空  
	linkMan联系人           非空  
	linkPhone联系号码       非空，手机号码格式正确  
2. 设置相关参数的默认值  
	 createMan创建人        当前登录用户名  
	 assignMan指派人  
		 如果未设置指派人（默认）  
			 state分配状态 （0=未分配，1=已分配）  
				 0 = 未分配  
					 assignTime指派时间  
					 设置为null  
			    devResult开发状态 （0=未开发，1=开发中，2=开发成功，3=开发失败）  
			         0 = 未开发 （默认）  
		如果设置了指派人  
			state分配状态 （0=未分配，1=已分配）  
				1 = 已分配  
			assignTime指派时间  
			   系统当前时间  
			devResult开发状态 （0=未开发，1=开发中，2=开发成功，3=开发失败）  
				1 = 开发中  
	 isValid是否有效  （0=无效，1=有效）  
	   设置为有效 1= 有效  
	createDate创建时间  
		默认是系统当前时间  
	updateDate  
		默认是系统当前时间  
3. 执行添加操作，判断受影响的行数

```java
/**  
 * 添加营销机会  
 * @param saleChance  
 * @return void  
 */
@Transactional(propagation = Propagation.REQUIRED)  
public void addSaleChance(SaleChance saleChance) {  
    /* 1. 校验参数 */    
    checkSaleChanceParams(saleChance.getCustomerName(), saleChance.getLinkMan(), saleChance.getLinkPhone());  
  
    /* 2. 设置相关字段的默认值 */    // isValid是否有效  （0=无效，1=有效） 设置为有效 1= 有效  
    saleChance.setIsValid(1);  
    // createDate创建时间 默认是系统当前时间  
    saleChance.setCreateDate(new Date());  
    // updateDate 默认是系统当前时间  
    saleChance.setUpdateDate(new Date());  
    // 判断是否设置了指派人  
    if(StringUtils.isBlank(saleChance.getAssignMan())) {  
        // 如果为空，则表示未设置指派人  
        //  state分配状态 （0=未分配，1=已分配）  0 = 未分配  
        saleChance.setState(StateStatus.UNSTATE.getType());  
        //  assignTime指派时间 设置为null  
        saleChance.setAssignTime(null);  
        // devResult开发状态 （0=未开发，1=开发中，2=开发成功，3=开发失败）  0 = 未开发 （默认）  
        saleChance.setDevResult(DevResult.UNDEV.getStatus());  
  
    } else {  
        // 如果不为空，则表示设置了指派人  
        // state分配状态 （0=未分配，1=已分配） 1 = 已分配  
        saleChance.setState(StateStatus.STATED.getType());  
        // assignTime指派时间 系统当前时间  
        saleChance.setAssignTime(new Date());  
        // devResult开发状态 （0=未开发，1=开发中，2=开发成功，3=开发失败）  1 = 开发中  
        saleChance.setDevResult(DevResult.DEVING.getStatus());  
  
    }  
  
    // 3. 执行添加操作，判断受影响的行数  
    AssertUtil.isTrue(saleChanceMapper.insertSelective(saleChance) != 1, "添加营销机会失败！");  
}

/**  
 *  参数校验  
 *      customerName客户名称    非空  
 *      linkMan联系人           非空  
 *      linkPhone联系号码       非空，手机号码格式正确  
 * @param customerName  
 * @param linkMan  
 * @param linkPhone  
 * @return void  
 */
private void checkSaleChanceParams(String customerName, String linkMan, String linkPhone) {  
    // customerName客户名称    非空  
    AssertUtil.isTrue(StringUtils.isBlank(customerName),"客户名称不能为空！");  
    // linkMan联系人           非空  
    AssertUtil.isTrue(StringUtils.isBlank(linkMan),"联系人不能为空！");  
    // linkPhone联系号码       非空  
    AssertUtil.isTrue(StringUtils.isBlank(linkPhone),"联系号码不能为空！");  
    // linkPhone联系号码       非空，手机号码格式正确  
    AssertUtil.isTrue(!PhoneUtil.isMobile(linkPhone),"联系号码格式不正确！");  
}
```

`saleChanceController`

```java
/**  
 * 添加营销机会 101002  
 * @param saleChance  
 * @return com.xxxx.crm.base.ResultInfo  
 */ 
@PostMapping("add")  
@ResponseBody  
public ResultInfo addSaleChance(SaleChance saleChance, HttpServletRequest request) {  
    // 从Cookie中获取当前登录的用户名  
    String userName = CookieUtil.getCookieValue(request, "userName");  
    // 设置用户名到营销机会对象  
    saleChance.setCreateMan(userName);  
    // 调用Service层的添加方法  
    saleChanceService.addSaleChance(saleChance);  
    return success("营销机会数据添加成功！");  
}
```

### 更新营销机会数据

`SaleChanceService`

`service`逻辑

1. 参数校验  
	营销机会ID  非空，数据库中对应的记录存在  
	customerName客户名称    非空  
	linkMan联系人           非空  
	linkPhone联系号码       非空，手机号码格式正确  
1. 设置相关参数的默认值  
		updateDate更新时间  设置为系统当前时间  
		assignMan指派人  
			原始数据未设置  
				修改后未设置  
					不需要操作  
				修改后已设置  
				assignTime指派时间  设置为系统当前时间  
					分配状态    1=已分配  
					开发状态    1=开发中  
				原始数据已设置  
					修改后未设置  
						assignTime指派时间  设置为原始的时间
						分配状态    0=未分配  
						开发状态    0=未开发  
					修改后已设置  
						判断修改前后是否是同一个指派人  
							如果是，则不需要操作  
							如果不是，则需要更新 assignTime指派时间  设置为系统当前时间  
3. 执行更新操作，判断受影响的行数  

```java
/**  
 * 更新营销机会  
 * @param saleChance  
 * @return void  
 */
@Transactional(propagation = Propagation.REQUIRED)  
public void updateSaleChance(SaleChance saleChance) {  
    /* 1. 参数校验  */    //  营销机会ID  非空，数据库中对应的记录存在  
    AssertUtil.isTrue(null == saleChance.getId(), "待更新记录不存在！");  
    // 通过主键查询对象  
    SaleChance temp = saleChanceMapper.selectByPrimaryKey(saleChance.getId());  
    // 判断数据库中对应的记录存在  
    AssertUtil.isTrue(temp == null,"待更新记录不存在！");  
    // 参数校验  
    checkSaleChanceParams(saleChance.getCustomerName(),saleChance.getLinkMan(), saleChance.getLinkPhone());  
  
  
    /* 2. 设置相关参数的默认值 */    // updateDate更新时间  设置为系统当前时间  
    saleChance.setUpdateDate(new Date());  
    // assignMan指派人  
    // 判断原始数据是否存在  
    if (StringUtils.isBlank(temp.getAssignMan())) { // 不存在  
        // 判断修改后的值是否存在  
        if (!StringUtils.isBlank(saleChance.getAssignMan())) { // 修改前为空，修改后有值  
            // assignTime指派时间  设置为系统当前时间  
            saleChance.setAssignTime(new Date());  
            // 分配状态    1=已分配  
            saleChance.setState(StateStatus.STATED.getType());  
            // 开发状态    1=开发中  
            saleChance.setDevResult(DevResult.DEVING.getStatus());  
        }  
    } else { // 存在  
        // 判断修改后的值是否存在  
        if (StringUtils.isBlank(saleChance.getAssignMan())) { // 修改前有值，修改后无值  
            // assignTime指派时间  设置为null  
            saleChance.setAssignTime(null);  
            // 分配状态    0=未分配  
            saleChance.setState(StateStatus.UNSTATE.getType());  
            // 开发状态    0=未开发  
            saleChance.setDevResult(DevResult.UNDEV.getStatus());  
        } else { // 修改前有值，修改后有值  
            // 判断修改前后是否是同一个用户  
            if (!saleChance.getAssignMan().equals(temp.getAssignMan())) {  
                // 更新指派时间  
                saleChance.setAssignTime(new Date());  
            } else {  
                // 设置指派时间为修改前的时间  
                saleChance.setAssignTime(temp.getAssignTime());  
            }  
        }  
    }  
  
    /* 3. 执行更新操作，判断受影响的行数 */    AssertUtil.isTrue(saleChanceMapper.updateByPrimaryKeySelective(saleChance) != 1, "更新营销机会失败！");  
  
}
```

`SaleChanceController`

```java
/**  
 * 更新营销机会 101004  
 * @param saleChance  
 * @return com.xxxx.crm.base.ResultInfo  
 */
@PostMapping("update")  
@ResponseBody  
public ResultInfo updateSaleChance(SaleChance saleChance) {  
    // 调用Service层的添加方法  
    saleChanceService.updateSaleChance(saleChance);  
    return success("营销机会数据更新成功！");  
}
```

### 删除营销机会数据

删除并不是真正的删除，仅仅是将数据设置为未启用

不论是单个删除还是批量删除，都将id以数组的方式传到后端进行统一删除

`SaleChanceService`

```java
/**  
 * 删除营销机会  
 *  
 * @param ids  
 * @return void  
 */
@Transactional(propagation = Propagation.REQUIRED)  
public void deleteSaleChance(Integer[] ids) {  
    // 判断ID是否为空  
    AssertUtil.isTrue(null == ids || ids.length < 1, "待删除记录不存在！");  
    // 执行删除（更新）操作，判断受影响的行数  
    AssertUtil.isTrue(saleChanceMapper.deleteBatch(ids) != ids.length, "营销机会数据删除失败！");  
}
```

`SaleChanceController`

**最好在前端对 `ids` 数组转为 `JSON` 字符串 `JSON.stringify(ids)`，设置 `contentType : "application/json"` （相应的后端参数需要设置`@RequestBody` 注解）。如果不设置传递 `JSON` 那么只能采用 `?ids=1&ids=2....` 方式**

```java
/**  
 * 删除营销机会 101003  
 * * @param ids  
 * @return com.xxxx.crm.base.ResultInfo  
 */
@PostMapping("delete")  
@ResponseBody  
public ResultInfo deleteSaleChance(Integer[] ids) {  // @RequestBody Integer[] ids
    // 调用Service层的删除方法  
    saleChanceService.deleteBatch(ids);  
    return success("营销机会数据删除成功！");  
}
```

自定义批量删除的`sql`

```xml
<!-- 批量删除（修改操作） -->  
<update id="deleteBatch">  
	update      
		t_sale_chance  
	set      
		is_valid = 0  
	where      
		id  
	in      
		<foreach collection="array" separator="," open="(" close=")" item="id">  
	        #{id}      
		</foreach>  
</update>
```


### 更新营销机会状态

`SaleChanceController`

```java
/**  
 * 更新营销机会的开发状态  
 *  
 * @param id  
 * @param devResult  
 * @return com.xxxx.crm.base.ResultInfo  
 */
@PostMapping("updateSaleChanceDevResult")  
@ResponseBody  
public ResultInfo updateSaleChanceDevResult(Integer id, Integer devResult) {  
  
    saleChanceService.updateSaleChanceDevResult(id, devResult);  
  
    return success("开发状态更新成功！");  
}
```

`SaleChanceService`

```java
/***  
 *更新营销机会的开发状态  
 * @param id  
 * @param devResult  
 * @return void  
 */
@Transactional(propagation = Propagation.REQUIRED)  
public void updateSaleChanceDevResult(Integer id, Integer devResult) {  
    // 判断ID是否为空  
    AssertUtil.isTrue(null == id, "待更新记录不存在！");  
    // 通过id查询营销机会数据  
    SaleChance saleChance = saleChanceMapper.selectByPrimaryKey(id);  
    // 判断对象是否为空  
    AssertUtil.isTrue(null == saleChance, "待更新记录不存在！");  
  
    // 设置开发状态  
    saleChance.setDevResult(devResult);  
  
    // 执行更新操作，判断受影响的函数  
    AssertUtil.isTrue(saleChanceMapper.updateByPrimaryKeySelective(saleChance) != 1, "开发状态更新失败！");  
}
```


## 客户开发计划

客户开发计划是在营销计划之内（分配状态为以分配且指派人是当前用户），给客户的相对应的计划内容，所以需要联合营销计划

### 客户开发计划数据查询

因为别个数据属于有效管理，开发计划是对于开发中，或有开发结果的数据进行显示详情：开发的计划表

`saleChanceService`

在营销机会数据查询中 `saleChanceService` 中添加如下判断（添加参数`Integer flag, HttpServletRequest request`）

```java
// 判断flag的值  
if (flag != null && flag == 1) {  
    // 查询客户开发计划  
    // 设置分配状态  
    saleChanceQuery.setState(StateStatus.STATED.getType());  
    // 设置指派人（当前登录用户的ID）  
    // 从cookie中获取当前登录用户的ID  
    Integer userId = LoginUserUtil.releaseUserIdFromCookie(request);  
    saleChanceQuery.setAssignMan(userId);  
}
```

获取当前营销机会的计划表（根据营销机会的`id`）

`CusDevPlanController`

```java
/***  
 * 客户开发计划数据查询（分页多条件查询）  
 *  
 * @param cusDevPlanQuery  
 * @return java.util.Map<java.lang.String,java.lang.Object>  
 */  
@RequestMapping("list")  
@ResponseBody  
public Map<String, Object> queryCusDevPlanByParams(CusDevPlanQuery cusDevPlanQuery) {  
    return cusDevPlanService.queryCUsDevPlanByParams(cusDevPlanQuery);  
}
```

`CusDevPlanService`

```java
/**  
 * 多条件分页查询客户开发计划
 *  
 * @param cusDevPlanQuery  
 * @return java.util.Map<java.lang.String,java.lang.Object>  
 */  
public Map<String, Object> queryCUsDevPlanByParams(CusDevPlanQuery cusDevPlanQuery) {  
  
    Map<String, Object> map = new HashMap<>();  
  
    // 开启分页  
    PageHelper.startPage(cusDevPlanQuery.getPage(), cusDevPlanQuery.getLimit());  
    // 得到对应分页对象  
    PageInfo<CusDevPlan> pageInfo = new PageInfo<>(cusDevPlanMapper.selectByParams(cusDevPlanQuery));  
  
    // 设置map对象  
    map.put("code",0);  
    map.put("msg","success");  
    map.put("count",pageInfo.getTotal());  
    // 设置分页好的列表  
    map.put("data",pageInfo.getList());  
  
    return map;  
}
```


### 添加客户开发计划

`CusDevPlanService

`service` 层逻辑

1. 参数校验  
	营销机会ID   非空，数据存在  
	计划项内容   非空  
	计划时间     非空  
2. 设置参数的默认值  
	是否有效    默认有效  
	创建时间    系统当前时间  
	修改时间    系统当前时间  
3. 执行添加操作，判断受影响的行数

```java
/***  
 * 添加客户开发计划项数据  
 * @param cusDevPlan  
 * @return void  
 */
@Transactional(propagation = Propagation.REQUIRED)  
public void addCusDevPlan(CusDevPlan cusDevPlan) {  
    /* 1. 参数校验  */    checkCusDevPlanParams(cusDevPlan);  
  
    /* 2. 设置参数的默认值 */    // 是否有效    默认有效  
    cusDevPlan.setIsValid(1);  
    // 创建时间    系统当前时间  
    cusDevPlan.setCreateDate(new Date());  
    // 修改时间    系统当前时间  
    cusDevPlan.setUpdateDate(new Date());  
  
    /* 3. 执行添加操作，判断受影响的行数 */    AssertUtil.isTrue(cusDevPlanMapper.insertSelective(cusDevPlan) != 1, "计划项数据添加失败！");  
}

/**  
 *  参数校验  
 *      营销机会ID   非空，数据存在  
 *      计划项内容   非空  
 *      计划时间     非空  
 * @param cusDevPlan  
 * @return void  
 */
private void checkCusDevPlanParams(CusDevPlan cusDevPlan) {  
    // 营销机会ID   非空，数据存在  
    Integer sId = cusDevPlan.getSaleChanceId();  
    AssertUtil.isTrue(null == sId || saleChanceMapper.selectByPrimaryKey(sId) == null,"数据异常，请重试！");  
  
    // 计划项内容   非空  
    AssertUtil.isTrue(StringUtils.isBlank(cusDevPlan.getPlanItem()), "计划项内容不能为空！");  
  
    // 计划时间     非空  
    AssertUtil.isTrue(null == cusDevPlan.getPlanDate(), "计划时间不能为空！");  
  
}
```

`CusDevPlanController`

```java
/**  
 * 添加计划项  
 *  
 * @param cusDevPlan  
 * @return com.xxxx.crm.base.ResultInfo  
 */
@PostMapping("add")  
@ResponseBody  
public ResultInfo addCusDevPlan(CusDevPlan cusDevPlan) {  
    cusDevPlanService.addCusDevPlan(cusDevPlan);  
    return success("计划项添加成功！");  
}
```

### 更新客户开发计划数据

`CusDevPlanService`

`service`逻辑

1. 参数校验  
	计划项ID     非空，数据存在  
	营销机会ID   非空，数据存在  
	计划项内容   非空  
	计划时间     非空  
2. 设置参数的默认值  
	修改时间    系统当前时间  
3. 执行更新操作，判断受影响的行数

```java
/**  
 * 更新营销机会  
 * @param saleChance  
 * @return void  
 */
/**  
 * 更新客户开发计划项数据  
 *  
 * @param cusDevPlan  
 * @return void  
 */
@Transactional(propagation = Propagation.REQUIRED)  
public void updateCusDevPlan(CusDevPlan cusDevPlan){  
    /* 1. 参数校验  */    // 计划项ID     非空，数据存在  
    AssertUtil.isTrue(null == cusDevPlan.getId()  
            || cusDevPlanMapper.selectByPrimaryKey(cusDevPlan.getId()) == null, "数据异常，请重试！");  
    checkCusDevPlanParams(cusDevPlan);  
  
    /* 2. 设置参数的默认值 */    // 修改时间    系统当前时间  
    cusDevPlan.setUpdateDate(new Date());  
  
    /* 3. 执行更新操作，判断受影响的行数 */    AssertUtil.isTrue(cusDevPlanMapper.updateByPrimaryKeySelective(cusDevPlan) != 1, "计划项更新失败！");  
  
}
```

`CusDevPlanController`

```java
/**  
 * 更新计划项  
 * @param cusDevPlan  
 * @return com.xxxx.crm.base.ResultInfo  
 */
@PostMapping("update")  
@ResponseBody  
public ResultInfo updateCusDevPlan(CusDevPlan cusDevPlan) {  
    cusDevPlanService.updateCusDevPlan(cusDevPlan);  
    return success("计划项更新成功！");  
}
```


### 删除客户开发计划数据

删除并不是真正的删除，仅仅是将数据设置为未启用

`CusDevPlanService`

```java
/***  
 * 删除计划项  
 *  1. 判断ID是否为空，且数据存在  
 *  2. 修改isValid属性  
 *  3. 执行更新操作  
 * @param id  
 * @return void  
 */
public void deleteCusDevPlan(Integer id) {  
    // 1. 判断ID是否为空，且数据存在  
    AssertUtil.isTrue(null == id, "待删除记录不存在！");  
    // 通过ID查询计划项对象  
    CusDevPlan cusDevPlan = cusDevPlanMapper.selectByPrimaryKey(id);  
    // 设置记录无效（删除）  
    cusDevPlan.setIsValid(0);  
    cusDevPlan.setUpdateDate(new Date());  
    // 执行更新操作  
    AssertUtil.isTrue(cusDevPlanMapper.updateByPrimaryKeySelective(cusDevPlan) != 1, "计划项数据删除失败！");  
}
```

`CusDevPlanController`

```java
/**  
 * 删除计划项  
 *  
 * @param id  
 * @return com.xxxx.crm.base.ResultInfo  
 */@DeleteMapping("delete")  
@ResponseBody  
public ResultInfo deleteCusDevPlan(Integer id) {  
    cusDevPlanService.deleteCusDevPlan(id);  
    return success("计划项更新成功！");  
}
```