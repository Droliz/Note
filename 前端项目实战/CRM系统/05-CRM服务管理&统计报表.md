# 05-CRM服务管理&统计报表

![](../../markdown_img/Pasted%20image%2020230213003633.png)

## 服务管理表结构设计

![](../../markdown_img/Pasted%20image%2020230117154223.png)

|t_customer_serve|客户服务表|||
|:--:|:--:|:--:|:--:|
||字段|字段类型|字段限制|字段描述|
|主键|id|int(11)|自增|id主键|
||serve_type|varchar(30)||客户编号|
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

## 服务管理

### 查询所有服务

`service`

```java
/**  
 * 多条件分页查询服务数据列表  
 *  
 * @param customerServeQuery  
 * @return java.util.Map<java.lang.String,java.lang.Object>  
 */  
public Map<String, Object> queryCustomerServeByParams(CustomerServeQuery customerServeQuery) {  
    Map<String, Object> map = new HashMap<>();  
  
    // 开启分页  
    PageHelper.startPage(customerServeQuery.getPage(), customerServeQuery.getLimit());  
    // 得到对应分页对象  
    PageInfo<CustomerServe> pageInfo = new PageInfo<>(customerServeMapper.selectByParams(customerServeQuery));  
  
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
 * 多条件分页查询服务数据的列表  
 *  
 * @param customerServeQuery  
 * @return java.util.Map<java.lang.String,java.lang.Object>  
 */  
@RequestMapping("list")  
@ResponseBody  
public Map<String,Object> queryCustomerServeByParams(CustomerServeQuery customerServeQuery,  
                                                     Integer flag, HttpServletRequest request) {  
  
    // 判断是否执行服务处理，如果是则查询分配给当前登录用户的服务记录  
    if (flag != null && flag == 1) {  
        // 设置查询条件：分配
        customerServeQuery.setAssigner(LoginUserUtil.releaseUserIdFromCookie(request));  
    }  
  
    return customerServeService.queryCustomerServeByParams(customerServeQuery);  
}
```


### 创建服务

`service`

添加服务操作  
1. 参数校验  
    客户名 customer  
        非空，客户表中存在客户记录  
    服务类型 serveType  
        非空  
    服务请求内容  serviceRequest  
        非空  
2. 设置参数的默认值  
    服务状态  
        服务创建状态  fw_001  
    是否有效  
    创建时间  
    更新时间  
    创建人 createPeople  
        （前端页面中通过cookie获取，传递到后台）  
2. 执行添加操作，判断受影响的行数  

```java
/**  
 *  
 * @param customerServe  
 * @return void  
 */
@Transactional(propagation = Propagation.REQUIRED)  
public void addCustomerServe(CustomerServe customerServe) {  
  
    /* 1. 参数校验 */    // 客户名 customer     非空  
    AssertUtil.isTrue(StringUtils.isBlank(customerServe.getCustomer()), "客户名不能为空！");  
    // 客户名 customer     客户表中存在客户记录  
    AssertUtil.isTrue(customerMapper.queryCustomerByName(customerServe.getCustomer()) == null, "客户不存在！" );  
  
    // 服务类型 serveType  非空  
    AssertUtil.isTrue(StringUtils.isBlank(customerServe.getServeType()), "请选择服务类型！");  
  
    //  服务请求内容  serviceRequest  非空  
    AssertUtil.isTrue(StringUtils.isBlank(customerServe.getServiceRequest()), "服务请求内容不能为空！");  
  
    /* 2. 设置参数的默认值 */    //  服务状态    服务创建状态  fw_001    customerServe.setState(CustomerServeStatus.CREATED.getState());  
    customerServe.setIsValid(1);  
    customerServe.setCreateDate(new Date());  
    customerServe.setUpdateDate(new Date());  
  
    /* 2. 执行添加操作，判断受影响的行数 */    AssertUtil.isTrue(customerServeMapper.insertSelective(customerServe) < 1, "添加服务失败！");  
}
```

`controller`

```java
/**  
 * 创建服务  
 *  
 * @param customerServe  
 * @return com.xxxx.crm.base.ResultInfo  
 */
@PostMapping("add")  
@ResponseBody  
public ResultInfo addCustomerServe(CustomerServe customerServe) {  
    customerServeService.addCustomerServe(customerServe);  
    return success("添加服务成功！");  
}
```

### 服务更新

`service`

 **服务分配/服务处理/服务反馈**  
 
 1. 参数校验与设置参数的默认值  
    客户服务ID  
        非空，记录必须存在  
    客户服务状态  
        如果服务状态为 服务分配状态 fw_002  
            分配人  
                非空，分配用户记录存在  
            分配时间  
                系统当前时间  
            更新时间  
                系统当前时间   
        如果服务状态为 服务处理状态 fw_003  
            服务处理内容  
                非空  
            服务处理时间  
                系统当前时间  
            更新时间  
                系统当前时间  
         如果服务状态是 服务反馈状态  fw_004  
            服务反馈内容  
                非空  
            服务满意度  
                非空  
            更新时间  
                系统当前时间  
            服务状态  
                设置为 服务归档状态 fw_005  
 2. 执行更新操作，判断受影响的行数  

```java
/**  
 *  
 * @param customerServe  
 * @return void  
 */
@Transactional(propagation = Propagation.REQUIRED)  
public void updateCustomerServe(CustomerServe customerServe) {  
    // 客户服务ID  非空且记录存在  
    AssertUtil.isTrue(customerServe.getId() == null  
            || customerServeMapper.selectByPrimaryKey(customerServe.getId()) == null, "待更新的服务记录不存在！");  
  
    // 判断客户服务的服务状态  
    if (CustomerServeStatus.ASSIGNED.getState().equals(customerServe.getState())) {  
        // 服务分配操作  
        // 分配人       非空，分配用户记录存在  
        AssertUtil.isTrue(StringUtils.isBlank(customerServe.getAssigner()), "待分配用户不能为空！");  
        AssertUtil.isTrue(userMapper.selectByPrimaryKey(Integer.parseInt(customerServe.getAssigner())) == null, "待分配用户不存在！");  
        // 分配时间     系统当前时间  
        customerServe.setAssignTime(new Date());  
  
  
    } else if (CustomerServeStatus.PROCED.getState().equals(customerServe.getState())) {  
        // 服务处理操作  
        // 服务处理内容   非空  
        AssertUtil.isTrue(StringUtils.isBlank(customerServe.getServiceProce()), "服务处理内容不能为空！");  
        // 服务处理时间   系统当前时间  
        customerServe.setServiceProceTime(new Date());  
  
    } else if (CustomerServeStatus.FEED_BACK.getState().equals(customerServe.getState())) {  
        // 服务反馈操作  
        // 服务反馈内容   非空  
        AssertUtil.isTrue(StringUtils.isBlank(customerServe.getServiceProceResult()), "服务反馈内容不能为空！");  
        // 服务满意度     非空  
        AssertUtil.isTrue(StringUtils.isBlank(customerServe.getMyd()), "请选择服务反馈满意度！");  
        // 服务状态      设置为 服务归档状态 fw_005        customerServe.setState(CustomerServeStatus.ARCHIVED.getState());  
  
    }  
  
    // 更新时间     系统当前时间  
    customerServe.setUpdateDate(new Date());  
  
    // 执行更新操作，判断受影响的行数  
    AssertUtil.isTrue(customerServeMapper.updateByPrimaryKeySelective(customerServe)< 1, "服务更新失败！");  
  
}
```

`controller`

```java
/**  
 * 服务更新  
 *     1. 服务分配  
 *     2. 服务处理  
 *     3.服务反馈  
 *  
 * @param customerServe  
 * @return com.xxxx.crm.base.ResultInfo  
 */
@PostMapping("update")  
@ResponseBody  
public ResultInfo updateCustomerServe(CustomerServe customerServe) {  
    customerServeService.updateCustomerServe(customerServe);  
    return success("服务更新成功！");  
}
```


## 统计报表

