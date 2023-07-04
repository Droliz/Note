# 04-CRM客户管理

![](../../markdown_img/Pasted%20image%2020230120182834.png)

## 表结构设计

### 客户信息管理表结构

t_customer 客户表、t_customer_contact 客户交往记录表、t_customer_linkman 客户联系人表t_ customer_ order 客户订单表、t_order_details 订单详情表

![](../../markdown_img/Pasted%20image%2020230117153942.png)

|t_customer|客户信息表||||
|:--:|:--:|:--:|:--:|:--|
||字段|字段类型|字段限制|字段描述|
|主键|id|int(11)|自增|id主键|
||khno|varchar(20)||客户编号|
||name|varchar(20)||客户姓名|
||area|varchar(20)||客户所属地区|
||cus_manager|varchar(20)||客户经理|
||level|varchar(30)||客户级别|
||myd|varchar(30)||客户满意度|
||xyd|varchar(30)||客户信用度|
||address|varchar(500)||客户地址|
||post_code|varchar(50)||邮编|
||phone|varchar(30)||联系电话|
||fax|varchar(30)||传真|
||web_site|varchar(20)||网址|
||yyzzzch|varchar(50)||营业执照注册号|
||fr|varchar(20)||法人代表|
||zczj|varchar(30)||注册资金|
||nyye|varchar(30)||年营业额|
||khyh|varchar(50)||开户银行|
||khzh|varchar(50)||开户账号|
||dsdjh|varchar(50)||地税登记号|
||gsdjh|varchar(50)||国税登记号|
||state|int(11)||流失状态|
||is_valid|int(11)||有效状态|
||create_date|datetime||创建时间|
||update_date|datetime||更新时间|


|t_customer_contact|客户交往记录表|
|:--:|:--:|:--:|:--:|:--|
||字段|字段类型|字段限制|字段描述|
|主键|id|int(11)|自增|id主键|
||cus_id|int(11)||客户id|
||link_name|varchar(20)||联系人姓名|
||sex|varchar(20)||性别|
||zhiwei|varchar(50)||职位|
||office_phone|varchar(50)||办公电话|
||phone|varchar(20)||手机号|
||is_valid|int(11)||有效状态|
||create_date|datetime||创建时间|
||update_date|datetime||更新时间|

|t_customer_order|客户订单表|
|:--:|:--:|:--:|:--:|:--|
||字段|字段类型|字段限制|字段描述|
|主键|id|int(11)|自增|id主键|
||cus_id|int(11)||客户id|
||order_no|varchar(40)||订单编号|
||order_data|datetime||下单时间|
||address|varchar(200)||地址|
||state|int(11)||状态|
||is_valid|int(11)||有效状态|
||create_date|datetime||创建时间|
||update_date|datetime||更新时间|

|t_customer_details|客户订单表|
|:--:|:--:|:--:|:--:|:--|
||字段|字段类型|字段限制|字段描述|
|主键|id|int(11)|自增|id主键|
||order_id|int(11)||订单id|
||goods_name|varchar(100)||商品名称|
||goods_num|int(11)||商品数量|
||unit|varchar(20)||商品单位|
||price|float||单价|
||sum|float||总金额|
||state|int(11)||状态|
||is_valid|int(11)||有效状态|
||create_date|datetime||创建时间|
||update_date|datetime||更新时间|

### 客户流失管理表结构

t_customer 客户表、t_customer_loss 客户流失表、t_customer_reprieve 客户流失暂缓表

![](../../markdown_img/Pasted%20image%2020230117154134.png)

|t_customer_loss|客户流失表|
|:--:|:--:|:--:|:--:|:--|
||字段|字段类型|字段限制|字段描述|
|主键|id|int(11)|自增|id主键|
||cus_no|varchar(40)||客户编号|
||cus_name|varchar(20)||客户姓名|
||cus_manager|varchar(20)||客户经理|
||last_order_time|datetime||最后下单时间|
||confirm_loss_time|datetime||确认流失时间|
||loss_reason|varchar(100)||流失原因|
||state|int(11)||流失状态|
||is_valid|int(11)||有效状态|
||create_date|datetime||创建时间|
||update_date|datetime||更新时间|

|t_customer_reprieve|客户流失暂缓表|
|:--:|:--:|:--:|:--:|:--|
||字段|字段类型|字段限制|字段描述|
|主键|id|int(11)|自增|id主键|
||loss_id|int(11)||流失id|
||measure|varchar(500)||措施|
||state|int(11)||流失状态|
||is_valid|int(11)||有效状态|
||create_date|datetime||创建时间|
||update_date|datetime||更新时间|

## 客户信息管理

### 客户信息查询

`CustomerService`

```java
/**  
 * 多条件分页查询客户 （返回的数据格式必须满足LayUi中数据表格要求的格式）  
 *  
 * @param customerQuery  
 * @return java.util.Map<java.lang.String,java.lang.Object>  
 */  
public Map<String, Object> queryCustomerByParams(CustomerQuery customerQuery) {  
  
    Map<String, Object> map = new HashMap<>();  
  
    // 开启分页  
    PageHelper.startPage(customerQuery.getPage(), customerQuery.getLimit());  
    // 得到对应分页对象  
    PageInfo<Customer> pageInfo = new PageInfo<>(customerMapper.selectByParams(customerQuery));  
  
    // 设置map对象  
    map.put("code",0);  
    map.put("msg","success");  
    map.put("count",pageInfo.getTotal());  
    // 设置分页好的列表  
    map.put("data",pageInfo.getList());  
  
    return map;  
}
```

`CustomerController`

```java
/**  
 * 分页条件查询客户列表  
 *  
 * @param customerQuery  
 * @return java.util.Map<java.lang.String,java.lang.Object>  
 */  
@RequestMapping("list")  
@ResponseBody  
public Map<String,Object> queryCustomerByParams(CustomerQuery customerQuery) {  
    return customerService.queryCustomerByParams(customerQuery);  
}
```

`sql`

```xml
<!-- 多条件查询 -->  
<select id="selectByParams" parameterType="com.xxxx.crm.query.CustomerQuery" resultType="com.xxxx.crm.vo.Customer">  
	select      
		<include refid="Base_Column_List"></include>  
	from      
		t_customer  
	<where>  
		is_valid = 1 and state = 0    
		<if test="null != customerName and customerName != ''">  
		  and name like concat('%',#{customerName},'%')    
		  </if>  
		<if test="null != customerNo and customerNo != ''">  
		  and khno = #{customerNo}    
		</if>  
		<if test="null != level and level != ''">  
		  and level = #{level}    
		</if>  
	</where>  
</select>
```



### 添加客户信息

`CustomerService`

`service`层逻辑

1. 参数校验  
    客户名称 name  
        非空，名称唯一  
    法人代表 fr  
        非空  
    手机号码 phone  
        非空，格式正确  
2. 设置参数的默认值  
    是否有效 isValid    1  
    创建时间 createDate 系统当前时间  
    修改时间 updateDate 系统当前时间  
    流失状态 state      0  
        0=正常客户  1=流失客户  
    客户编号 khno  
        系统生成，唯一 （uuid | 时间戳 | 年月日时分秒 | 雪花算法）  
        格式：KH + 时间戳  
3. 执行添加操作，判断受影响的行数  


```java
/**  
 * 添加客户  
 *  
 * @param customer  
 * @return void  
 */@Transactional(propagation = Propagation.REQUIRED)  
public void addCustomer(Customer customer) {  
    /* 1. 参数校验 */    
    checkCustomerParams(
	    customer.getName(), 
	    customer.getFr(), 
	    customer.getPhone());  
    // 判断客户名的唯一性  
    Customer temp = customerMapper
	    .queryCustomerByName(customer.getName());  
    // 判断名客户名称是否存在  
    AssertUtil.isTrue(null != temp, "客户名称已存在，请重新输入！");  
  
    /* 2. 设置参数的默认值 */    
    customer.setIsValid(1);  
    customer.setCreateDate(new Date());  
    customer.setUpdateDate(new Date());  
    customer.setState(0);  
    // 客户编号  
    String khno = "KH" + System.currentTimeMillis();  
    customer.setKhno(khno);  
  
    /* 3. 执行添加操作，判断受影响的行数 */    AssertUtil.isTrue(customerMapper.insertSelective(customer) < 1, "添加客户信息失败！");  
}

/**  
 * 参数校验  
 *      客户名称 name  
 *          非空  
 *      法人代表 fr  
 *          非空  
 *      手机号码 phone  
 *          非空，格式正确  
 *  
 * @param name  
 * @param fr  
 * @param phone  
 * @return void  
 */
private void checkCustomerParams(String name, String fr, String phone) {  
    // 客户名称 name    非空  
    AssertUtil.isTrue(StringUtils.isBlank(name), "客户名称不能为空！");  
    // 法人代表 fr      非空  
    AssertUtil.isTrue(StringUtils.isBlank(fr), "法人代表不能为空！");  
    // 手机号码 phone   非空  
    // AssertUtil.isTrue(StringUtils.isBlank(phone),"手机号码不能为空！");  
    // 手机号码 phone   格式正确  
    AssertUtil.isTrue(!PhoneUtil.isMobile(phone), "手机号码格式不正确！");  
}
```

`sql`

```xml
<!-- 通过客户名称查询客户对象 -->  
<select id="queryCustomerByName" parameterType="string" resultType="com.xxxx.crm.vo.Customer">  
	select      
	  <include refid="Base_Column_List"></include>  
	from      
	  t_customer  
	where      
		is_valid = 1 
	and 
		name = #{name}
</select>


```

`CustomerController`

```java
/**  
 * 添加客户信息  
 *  
 * @param customer  
 * @return com.xxxx.crm.base.ResultInfo  
 */
@PostMapping("add")  
@ResponseBody  
public ResultInfo addCustomer(Customer customer) {  
    customerService.addCustomer(customer);  
    return success("添加客户信息成功！");  
}
```

### 更新用户信息

`sql`

```xml
<update id="updateByPrimaryKeySelective" parameterType="com.xxxx.crm.vo.Customer" >  
  update t_customer  
  <set >  
    <if test="khno != null" >  
      khno = #{khno,jdbcType=VARCHAR},    </if>  
    <if test="name != null" >  
      name = #{name,jdbcType=VARCHAR},    </if>  
    <if test="area != null" >  
      area = #{area,jdbcType=VARCHAR},    </if>  
    <if test="cusManager != null" >  
      cus_manager = #{cusManager,jdbcType=VARCHAR},    </if>  
    <if test="level != null" >  
      level = #{level,jdbcType=VARCHAR},    </if>  
    <if test="myd != null" >  
      myd = #{myd,jdbcType=VARCHAR},    </if>  
    <if test="xyd != null" >  
      xyd = #{xyd,jdbcType=VARCHAR},    </if>  
    <if test="address != null" >  
      address = #{address,jdbcType=VARCHAR},    </if>  
    <if test="postCode != null" >  
      post_code = #{postCode,jdbcType=VARCHAR},    </if>  
    <if test="phone != null" >  
      phone = #{phone,jdbcType=VARCHAR},    </if>  
    <if test="fax != null" >  
      fax = #{fax,jdbcType=VARCHAR},    </if>  
    <if test="webSite != null" >  
      web_site = #{webSite,jdbcType=VARCHAR},    </if>  
    <if test="yyzzzch != null" >  
      yyzzzch = #{yyzzzch,jdbcType=VARCHAR},    </if>  
    <if test="fr != null" >  
      fr = #{fr,jdbcType=VARCHAR},    </if>  
    <if test="zczj != null" >  
      zczj = #{zczj,jdbcType=VARCHAR},    </if>  
    <if test="nyye != null" >  
      nyye = #{nyye,jdbcType=VARCHAR},    </if>  
    <if test="khyh != null" >  
      khyh = #{khyh,jdbcType=VARCHAR},    </if>  
    <if test="khzh != null" >  
      khzh = #{khzh,jdbcType=VARCHAR},    </if>  
    <if test="dsdjh != null" >  
      dsdjh = #{dsdjh,jdbcType=VARCHAR},    </if>  
    <if test="gsdjh != null" >  
      gsdjh = #{gsdjh,jdbcType=VARCHAR},    </if>  
    <if test="state != null" >  
      state = #{state,jdbcType=INTEGER},    </if>  
    <if test="isValid != null" >  
      is_valid = #{isValid,jdbcType=INTEGER},    </if>  
    <if test="createDate != null" >  
      create_date = #{createDate,jdbcType=TIMESTAMP},    </if>  
    <if test="updateDate != null" >  
      update_date = #{updateDate,jdbcType=TIMESTAMP},    </if>  
  </set>  
  where 
	  id = #{id,jdbcType=INTEGER}
</update>
```

`service`

1. 参数校验  
    客户ID id  
         非空，数据存在  
    客户名称 name  
        非空，名称唯一  
     法人代表 fr  
        非空  
    手机号码 phone  
        非空，格式正确  
2. 设置参数的默认值  
    修改时间 updateDate 系统当前时间  
3. 执行更新操作，判断受影响的行数 

```java
/**  
 * 修改客户  
 * 
 * @param customer  
 * @return void  
 */
@Transactional(propagation = Propagation.REQUIRED)  
public void updateCustomer(Customer customer) {  
    /* 1. 参数校验 */    
    AssertUtil.isTrue(null == customer.getId(), "未填写客户id！");  
    // 通过客户ID查询客户记录  
    Customer temp = customerMapper.selectByPrimaryKey(customer.getId());  
    // 判断客户记录是否存在  
    AssertUtil.isTrue(null == temp, "待更新记录不存在！");  
    // 参数校验  
    checkCustomerParams(customer.getName(), customer.getFr(), customer.getPhone());  
    // 通过客户名称查询客户记录  
    temp = customerMapper.queryCustomerByName(customer.getName());  
    // 判断客户记录 是否存在，且客户id是否与更新记录的id一致  
    AssertUtil.isTrue(null != temp && !(temp.getId()).equals(customer.getId()), "客户名称已存在，请重新输入！");  
  
    /* 2. 设置参数的默认值  */    
    customer.setUpdateDate(new Date());  
  
    /* 3. 执行更新操作，判断受影响的行数 */
    AssertUtil.isTrue(customerMapper.updateByPrimaryKeySelective(customer) < 1, "修改客户信息失败！");  
}
```

`CustomerController`

```java
/**  
 * 修改客户信息  
 *  
 * @param customer  
 * @return com.xxxx.crm.base.ResultInfo  
 */
@PostMapping("update")  
@ResponseBody  
public ResultInfo updateCustomer(Customer customer) {  
    customerService.updateCustomer(customer);  
    return success("修改客户信息成功！");  
}
```

### 删除客户信息

`service`

1. 参数校验  
    id  
        非空，数据存在  
2. 设置参数默认值  
    isValid     0  
    updateDate  系统当前时间  
3. 执行删除（更新）操作，判断受影响的行数  

```java
/**  
 * 删除客户  
 * @param id  
 * @return void  
 */
@Transactional(propagation = Propagation.REQUIRED)  
public void deleteCustomer(Integer id) {  
    // 判断id是否为空，数据是否存在  
    AssertUtil.isTrue(null == id, "待删除记录不存在！");  
    // 通过id查询客户记录  
    Customer customer = customerMapper.selectByPrimaryKey(id);  
    AssertUtil.isTrue(null == customer, "待删除记录不存在！");  
  
    // 设置状态为失效  
    customer.setIsValid(0);  
    customer.setUpdateDate(new Date());  
  
    // 执行删除（更新）操作，判断受影响的行数  
    AssertUtil.isTrue(customerMapper.updateByPrimaryKeySelective(customer) < 1, "删除客户信息失败！");  
  
}
```

`controller`

```java
/**  
 * 删除客户信息  
 *  
 * @param id  
 * @return com.xxxx.crm.base.ResultInfo  
 */
@PostMapping("delete")  
@ResponseBody  
public ResultInfo deleteCustomer(Integer id) {  
    customerService.deleteCustomer(id);  
    return success("删除客户信息成功！");  
}
```

## 客户订单管理

### 客户订单查看

`service`

```java
/**  
 * 多条件分页查询客户  
 *  
 * @param customerOrderQuery  
 * @return java.util.Map<java.lang.String,java.lang.Object>  
 */  
public Map<String, Object> queryCustomerOrderByParams(CustomerOrderQuery customerOrderQuery) {  
    Map<String, Object> map = new HashMap<>();  
  
    // 开启分页  
    PageHelper.startPage(customerOrderQuery.getPage(), customerOrderQuery.getLimit());  
    // 得到对应分页对象  
    PageInfo<CustomerOrder> pageInfo = new PageInfo<>(customerOrderMapper.selectByParams(customerOrderQuery));  
  
    // 设置map对象  
    map.put("code",0);  
    map.put("msg","success");  
    map.put("count",pageInfo.getTotal());  
    // 设置分页好的列表  
    map.put("data",pageInfo.getList());  
  
    return map;  
}
```

`controller`

```java
/**  
 * 分页多条件查询客户订单列表  
 *  
 * @param  
 * @return java.util.Map<java.lang.String,java.lang.Object>  
 */  
@RequestMapping("list")  
@ResponseBody  
public Map<String,Object> queryCustomerOrderByParams(CustomerOrderQuery customerOrderQuery) {  
    return customerOrderService.queryCustomerOrderByParams(customerOrderQuery);  
}
```

### 订单详情查看

`OrderDetailsService`

```java
/***  
 * 分页条件查询订单详情列表  
 *  
 * @param orderDetailsQuery  
 * @return java.util.Map<java.lang.String,java.lang.Object>  
 */  
public Map<String, Object> queryOrderDetailsByParams(OrderDetailsQuery orderDetailsQuery) {  
    Map<String, Object> map = new HashMap<>();  
  
    // 开启分页  
    PageHelper.startPage(orderDetailsQuery.getPage(), orderDetailsQuery.getLimit());  
    // 得到对应分页对象  
    PageInfo<OrderDetails> pageInfo = new PageInfo<>(orderDetailsMapper.selectByParams(orderDetailsQuery));  
  
    // 设置map对象  
    map.put("code",0);  
    map.put("msg","success");  
    map.put("count",pageInfo.getTotal());  
    // 设置分页好的列表  
    map.put("data",pageInfo.getList());  
  
    return map;  
}
```

`OrderDetailsController`

```java
/**  
 * 分页条件查询订单详情列表  
 *  
 * @param orderDetailsQuery  
 * @return java.util.Map<java.lang.String,java.lang.Object>  
 */  
@RequestMapping("list")  
@ResponseBody  
public Map<String,Object> queryOrderDetailsByParams(OrderDetailsQuery orderDetailsQuery) {  
    return orderDetailsService.queryOrderDetailsByParams(orderDetailsQuery);  
}
```

## 客户流失管理

流失客户定义：客户自创建起，超过六个月未与企业产生过任何订单（最后一笔订单在六个月之前）

### 更新客户的流失状态  

`CustomerService`

*  1. 查询待流失的客户数据  
*  2. 将流失客户数据批量添加到客户流失表中  
*  3. 批量更新客户的流失状态  0=正常客户  1=流失客户  

```java
/**  
 * @param  
 * @return void  
 */
@Transactional(propagation = Propagation.REQUIRED)  
public void updateCustomerState() {  
    /* 1. 查询待流失的客户数据 */    List<Customer> lossCustomerList = customerMapper.queryLossCustomers();  
  
    /* 2. 将流失客户数据批量添加到客户流失表中 */    // 判断流失客户数据是否存在  
    if (lossCustomerList != null && lossCustomerList.size() > 0) {  
        // 定义集合 用来接收流失客户的ID  
        List<Integer> lossCustomerIds = new ArrayList<>();  
        // 定义流失客户的列表  
        List<CustomerLoss> customerLossList = new ArrayList<>();  
        // 遍历查询到的流失客户的数据  
        lossCustomerList.forEach(customer -> {  
            // 定义流失客户对象  
            CustomerLoss customerLoss = new CustomerLoss();  
            // 创建时间  系统当前时间  
            customerLoss.setCreateDate(new Date());  
            // 客户经理  
            customerLoss.setCusManager(customer.getCusManager());  
            // 客户名称  
            customerLoss.setCusName(customer.getName());  
            // 客户编号  
            customerLoss.setCusNo(customer.getKhno());  
            // 是否有效  1=有效  
            customerLoss.setIsValid(1);  
            // 修改时间  系统当前时间  
            customerLoss.setUpdateDate(new Date());  
            // 客户流失状态   0=暂缓流失状态  1=确认流失状态  
            customerLoss.setState(0);  
            // 客户最后下单时间  
            // 通过客户ID查询最后的订单记录（最后一条订单记录）  
            CustomerOrder customerOrder = customerOrderMapper.queryLossCustomerOrderByCustomerId(customer.getId());  
            // 判断客户订单是否存在，如果存在，则设置最后下单时间  
            if (customerOrder != null) {  
                customerLoss.setLastOrderTime(customerOrder.getOrderDate());  
            }  
            // 将流失客户对象设置到对应的集合中  
            customerLossList.add(customerLoss);  
  
            // 将流失客户的ID设置到对应的集合中  
            lossCustomerIds.add(customer.getId());  
        });  
        // 批量添加流失客户记录  
        AssertUtil.isTrue(customerLossMapper.insertBatch(customerLossList) != customerLossList.size(), "客户流失数据转移失败！");  
  
        /* 3. 批量更新客户的流失状态 */        AssertUtil.isTrue(customerMapper.updateCustomerStateByIds(lossCustomerIds) != lossCustomerIds.size(), "客户流失数据转移失败！");  
    }  
  
}
```

定时任务：每隔 $2h$ 执行（$task$ 包下定义`jobTask`）

```java
package com.xxxx.crm.task;  
  
import com.xxxx.crm.service.CustomerService;  
import org.springframework.scheduling.annotation.Scheduled;  
import org.springframework.stereotype.Component;  
  
import javax.annotation.Resource;  
import java.text.SimpleDateFormat;  
import java.util.Date;  
  
/**  
 * 定时任务的执行  
 */  
@Component  
public class JobTask {  
  
    @Resource  
    private CustomerService customerService;  
  
  
    /**  
     * 每月第一天凌晨2点执行  
     *  
     * @param  
     * @return void  
     */     
    @Scheduled(cron = "0 0 2 1 * ? *")  
//    @Scheduled(cron = "0/2 * * * * ?")  // 每 2s 执行  
    public void job() {  
        System.out.println("定时任务开始执行 --> " + new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(new Date()));  
        // 调用需要被定时执行的方法  
        customerService.updateCustomerState();  
    }  
  
}
```

在启动类中添加注解来启动定时任务（`@EnableScheduling // 启用定时任务`）

### 流失客户查询

`service`

```java
/**  
 * 分页条件查询  
 *  
 * @param customerLossQuery  
 * @return java.util.Map<java.lang.String,java.lang.Object>  
 */  
public Map<String, Object> queryCustomerLossByParams(CustomerLossQuery customerLossQuery) {  
  
    Map<String, Object> map = new HashMap<>();  
  
    // 开启分页  
    PageHelper.startPage(customerLossQuery.getPage(), customerLossQuery.getLimit());  
    // 得到对应分页对象  
    PageInfo<CustomerLoss> pageInfo = new PageInfo<>(customerLossMapper.selectByParams(customerLossQuery));  
  
    // 设置map对象  
    map.put("code",0);  
    map.put("msg","success");  
    map.put("count",pageInfo.getTotal());  
    // 设置分页好的列表  
    map.put("data",pageInfo.getList());  
  
    return map;  
}
```

`controller`

```java
/**  
 * 分页条件查询流失客户列表  
 *  
 * @param customerLossQuery  
 * @return java.util.Map<java.lang.String,java.lang.Object>  
 */  
@GetMapping("list")  
@ResponseBody  
public Map<String, Object> queryCustomerLossByParams(CustomerLossQuery customerLossQuery) {  
    return customerLossService.queryCustomerLossByParams(customerLossQuery);  
}
```

### 获取暂缓列表

`LossService`

```java
/**  
 * 分页条件查询流失客户暂缓操作的列表  
 *  
 * @param customerReprieveQuery  
 * @return java.util.Map<java.lang.String,java.lang.Object>  
 */  
public Map<String, Object> queryCustomerReprieveByParams(CustomerReprieveQuery customerReprieveQuery) {  
    Map<String, Object> map = new HashMap<>();  
  
    // 开启分页  
    PageHelper.startPage(customerReprieveQuery.getPage(), customerReprieveQuery.getLimit());  
    // 得到对应分页对象  
    PageInfo<CustomerReprieve> pageInfo = new PageInfo<>(customerReprieveMapper.selectByParams(customerReprieveQuery));  
  
    // 设置map对象  
    map.put("code",0);  
    map.put("msg","success");  
    map.put("count",pageInfo.getTotal());  
    // 设置分页好的列表  
    map.put("data",pageInfo.getList());  
  
    return map;  
}
```

`controller`

```java
/**  
 * 分页条件查询流失客户暂缓操作的列表  
 *  
 * @param customerReprieveQuery  
 * @return java.util.Map<java.lang.String,java.lang.Object>  
 */  
@RequestMapping("list")  
@ResponseBody  
public Map<String, Object> queryCustomerReprieveByParams(CustomerReprieveQuery customerReprieveQuery) {  
    return customerReprieveService.queryCustomerReprieveByParams(customerReprieveQuery);  
}
```

### 增加暂缓操作

`service`

添加暂缓数据  
1. 参数校验  
    流失客户ID  lossId  
        非空，数据存在  
    暂缓措施内容 measure  
        非空  
2. 设置参数的默认值  
    是否有效  
        默认有效，1  
    创建时间  
        系统当前时间  
    修改时间  
        系统当前时间  
3. 执行添加操作，判断受影响的行数  

```java
/**  
 * @param customerReprieve  
 * @return void  
 */
@Transactional(propagation = Propagation.REQUIRED)  
public void addCustomerRepr(CustomerReprieve customerReprieve) {  
  
    /* 1. 参数校验 */    checkParams(customerReprieve.getLossId(), customerReprieve.getMeasure());  
  
    /* 2. 设置参数的默认值 */    customerReprieve.setIsValid(1);  
    customerReprieve.setCreateDate(new Date());  
    customerReprieve.setUpdateDate(new Date());  
  
    /* 3. 执行添加操作，判断受影响的行数 */    AssertUtil.isTrue(customerReprieveMapper.insertSelective(customerReprieve) < 1, "添加暂缓数据失败！");  
}

/**  
 * 参数校验  
 *  
 * @param lossId  
 * @param measure  
 * @return void  
 */
private void checkParams(Integer lossId, String measure) {  
    // 流失客户ID lossId    非空，数据存在  
    AssertUtil.isTrue(null == lossId  
            || customerLossMapper.selectByPrimaryKey(lossId) == null, "流失客户记录不存在！");  
    // 暂缓措施内容 measure   非空  
    AssertUtil.isTrue(StringUtils.isBlank(measure), "暂缓措施内容不能为空！");  
  
}
```

`controller`

```java
/**  
 * 添加暂缓数据  
 *  
 * @param customerReprieve  
 * @return com.xxxx.crm.base.ResultInfo  
 */
@PostMapping("add")  
@ResponseBody  
public ResultInfo addCustomerRepr(CustomerReprieve customerReprieve) {  
    customerReprieveService.addCustomerRepr(customerReprieve);  
    return success("添加暂缓数据成功！");  
}
```

### 更新暂缓操作

`service`

修改暂缓数据  
1. 参数校验  
    主键ID    id  
        非空，数据存在  
    流失客户ID  lossId  
        非空，数据存在  
    暂缓措施内容 measure  
        非空  
2. 设置参数的默认值  
    修改时间  
        系统当前时间  
3. 执行修改操作，判断受影响的行数  

```java
/**  
 *  
 * @param customerReprieve  
 * @return void  
 */@Transactional(propagation = Propagation.REQUIRED)  
public void updateCustomerRepr(CustomerReprieve customerReprieve) {  
    /* 1. 参数校验 */    // 主键ID    id  
    AssertUtil.isTrue(null == customerReprieve.getId()  
    || customerReprieveMapper.selectByPrimaryKey(customerReprieve.getId()) == null, "待更新记录不存在！");  
    // 参数校验  
    checkParams(customerReprieve.getLossId(), customerReprieve.getMeasure());  
  
    /* 2. 设置参数的默认值 */    customerReprieve.setUpdateDate(new Date());  
  
    /* 3. 执行修改操作，判断受影响的行数 */    AssertUtil.isTrue(customerReprieveMapper.updateByPrimaryKeySelective(customerReprieve) < 1, "修改暂缓数据失败！");  
  
}
```

`controller`

```java
/**  
 * 修改暂缓数据  
 *  
 * @param customerReprieve  
 * @return com.xxxx.crm.base.ResultInfo  
 */
@PostMapping("update")  
@ResponseBody  
public ResultInfo updateCustomerRepr(CustomerReprieve customerReprieve) {  
    customerReprieveService.updateCustomerRepr(customerReprieve);  
    return success("修改暂缓数据成功！");  
}
```

### 删除暂缓数据

`service`

删除暂缓数据  
1. 判断id是否为空，且数据存在  
2. 判断数据是否已流失  
3. 设置isvalid 为 0  
4. 执行更新操作，判断受影响的行数  

```java
/**  
 *  
 * @param id  
 * @return void  
 */
@Transactional(propagation = Propagation.REQUIRED)  
public void deleteCustomerRepr(Integer id) {  
    // 判断id是否为空  
    AssertUtil.isTrue(null == id, "待删除记录不存在！");  
    // 通过id查询暂缓数据  
    CustomerReprieve customerReprieve = customerReprieveMapper.selectByPrimaryKey(id);  
    // 判断数据是否存在  
    AssertUtil.isTrue(null == customerReprieve, "待删除记录不存在！");  
    // 判断数据是否已流失  
    AssertUtil.isTrue(customerReprieveMapper.isReprieve(id), "客户已流失，无法删除记录");  
    // 设置isValid  
    customerReprieve.setIsValid(0);  
    customerReprieve.setUpdateDate(new Date());  
  
    // 执行更新操作，判断受影响的行数  
    AssertUtil.isTrue(customerReprieveMapper.updateByPrimaryKeySelective(customerReprieve) < 1, "删除暂缓数据失败！");  
}
```

`controller`

```java
/**  
 * 删除暂缓数据（删除前判断是否已标记流失）  
 *  
 * @param id  
 * @return com.xxxx.crm.base.ResultInfo  
 */
@PostMapping("delete")  
@ResponseBody  
public ResultInfo updateCustomerRepr(Integer id) {  
    customerReprieveService.deleteCustomerRepr(id);  
    return success("删除暂缓数据成功！");  
}
```

### 标记为流失状态

`service`

更新流失客户的流失状态  
1. 参数校验  
    判断id非空且对应的数据存在  
    流失原因非空  
2. 设置参数的默认值  
    设置流失状态  state=1  0=暂缓流失，1=确认流失  
    流失原因  
    客户流失时间  系统当前时间  
    更新时间     系统当前时间  
3. 执行更新操作，判断受影响的行数  

```java
/**  
 *  
 * @param id  
 * @param lossReason  
 * @return void  
 */
@Transactional(propagation = Propagation.REQUIRED)  
public void updateCustomerLossStateById(Integer id, String lossReason) {  
    /* 1. 参数校验 */    // 判断id非空  
    AssertUtil.isTrue(null == id, "待确认流失的客户不存在！");  
    // 通过id查询流失客户的记录  
    CustomerLoss customerLoss = customerLossMapper.selectByPrimaryKey(id);  
    // 判断流失客户记录是否存在  
    AssertUtil.isTrue(null == customerLoss, "待确认流失的客户不存在！");  
    // 流失原因非空  
    AssertUtil.isTrue(StringUtils.isBlank(lossReason), "流失原因不能为空！");  
  
    /* 2. 设置参数的默认值 */    // 设置流失状态  state=1  0=暂缓流失，1=确认流失  
    customerLoss.setState(1);  
    // 设置流失原因  
    customerLoss.setLossReason(lossReason);  
    // 客户流失时间  系统当前时间  
    customerLoss.setConfirmLossTime(new Date());  
    // 更新时间     系统当前时间  
    customerLoss.setUpdateDate(new Date());  
  
    /* 3. 执行更新操作，判断受影响的行数 */    AssertUtil.isTrue(customerLossMapper.updateByPrimaryKeySelective(customerLoss) < 1, "确认流失失败！");  
}
```

`controller`

```java
/**  
 * 更新流失客户的流失状态  
 *  
 * @param  
 * @return com.xxxx.crm.base.ResultInfo  
 */
@PostMapping("updateCustomerLossStateById")  
@ResponseBody  
public ResultInfo updateCustomerLossStateById(Integer id, String lossReason) {  
    customerLossService.updateCustomerLossStateById(id, lossReason);  
    return success("确认流失成功！");  
}
```

