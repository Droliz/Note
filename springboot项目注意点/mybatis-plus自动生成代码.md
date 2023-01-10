# mybatis-plus自动生成代码

## 代码

```java
package com.qtw.common;  
  
import com.baomidou.mybatisplus.core.exceptions.MybatisPlusException;  
import com.baomidou.mybatisplus.core.toolkit.StringPool;  
import com.baomidou.mybatisplus.core.toolkit.StringUtils;  
import com.baomidou.mybatisplus.generator.AutoGenerator;  
import com.baomidou.mybatisplus.generator.InjectionConfig;  
import com.baomidou.mybatisplus.generator.config.*;  
import com.baomidou.mybatisplus.generator.config.po.TableInfo;  
import com.baomidou.mybatisplus.generator.config.rules.NamingStrategy;  
import com.baomidou.mybatisplus.generator.engine.FreemarkerTemplateEngine;  
  
import java.util.ArrayList;  
import java.util.List;  
import java.util.Scanner;  
  
// 生成代码  
public class CodeGenerator {  
  
    /**  
     * <p>  
     * 读取控制台内容  
     * </p>  
     */  
    public static String scanner(String tip) {  
        Scanner scanner = new Scanner(System.in);  
        System.out.println("请输入" + tip + "：");  
        if (scanner.hasNext()) {  
            String ipt = scanner.next();  
            if (StringUtils.isNotBlank(ipt)) {  
                return ipt;  
            }  
        }  
        throw new MybatisPlusException("请输入正确的" + tip + "！");  
    }  
  
    /*  
    * 操作步骤：  
    * 1、修改数据源，包括地址密码信息  
    * 2、模板配置，可以修改包名  
    * 3、修改米板  
    * */  
    public static void main(String[] args) {  
        // 代码生成器  
        AutoGenerator mpg = new AutoGenerator();  
  
        // 全局配置  
        GlobalConfig gc = new GlobalConfig();  
        String projectPath = System.getProperty("user.dir");  // 项目根路径  
        gc.setOutputDir(projectPath + "/src/main/java");  
        gc.setAuthor("qtw");  
        gc.setOpen(false);  
        gc.setSwagger2(true); // 实体属性 Swagger2 注解  
        gc.setBaseResultMap(true);  // XML ResultMap  
        gc.setBaseColumnList(true);  // XML columList  
        // 去掉service接口首字母的 I        
        gc.setServiceName("%sService");  
        mpg.setGlobalConfig(gc);  
  
        // 数据源配置  
        DataSourceConfig dsc = new DataSourceConfig();  
        dsc.setUrl("jdbc:mysql://localhost:3306/test?useUnicode=true&useSSL=false&characterEncoding=utf8");  
        // dsc.setSchemaName("public");  
        dsc.setDriverName("com.mysql.jdbc.Driver");  
        dsc.setUsername("root");  
        dsc.setPassword("123456");  
        mpg.setDataSource(dsc);  
  
        // 包配置  
        PackageConfig pc = new PackageConfig();  
		// pc.setModuleName(scanner("模块名"));  
        // 模块配置  
        pc.setParent("com.qtw")  
                .setEntity("entity")  
                .setMapper("mapper")  
                .setService("service")  
                .setServiceImpl("service.impl")  
                .setController("controller");  
        mpg.setPackageInfo(pc);  
  
        // 自定义配置  
        InjectionConfig cfg = new InjectionConfig() {  
            @Override  
            public void initMap() {  
                // to do nothing  
            }  
        };  
  
        // 如果模板引擎是 freemarker        
        String templatePath = "/templates/mapper.xml.ftl";  
        // 如果模板引擎是 velocity        
        // String templatePath = "/templates/mapper.xml.vm";  
        // 自定义输出配置  
        List<FileOutConfig> focList = new ArrayList<>();  
        // 自定义配置会被优先输出  
        focList.add(new FileOutConfig(templatePath) {  
            @Override  
            public String outputFile(TableInfo tableInfo) {  
                // 自定义输出文件名 ， 如果你 Entity 设置了前后缀、此处注意 xml 的名称会跟着发生变化！！  
                return projectPath + "/src/main/resources/mapper/" + pc.getModuleName()  
                        + "/" + tableInfo.getEntityName() + "Mapper" + StringPool.DOT_XML;  
            }  
        });  
  
        /*  
        cfg.setFileCreate(new IFileCreate() {            
	        @Override            
	        public boolean isCreate(
		        ConfigBuilder configBuilder, FileType fileType, String filePath) {                
		        // 判断自定义文件夹是否需要创建  
                checkDir("调用默认方法创建的目录，自定义目录用");  
                if (fileType == FileType.MAPPER) {                    
					// 已经生成 mapper 文件判断存在，不想重新生成返回 false                   
					return !new File(filePath).exists();                
				}                
				// 允许生成模板文件  
                return true;            
            }        
        }); */
          
        cfg.setFileOutConfigList(focList);  
        mpg.setCfg(cfg);  
  
        // 配置模板  
        TemplateConfig templateConfig = new TemplateConfig();  
  
        // 配置自定义输出模板  
        //指定自定义模板路径，注意不要带上.ftl/.vm, 会根据使用的模板引擎自动识别  
        // 修改模板  
        // templateConfig.setEntity("templates/entity2.java");  
        // templateConfig.setService("templates/service2.java");        
        // templateConfig.setController("templates/controller2.java");        
        // templateConfig.setMapper("templates/mapper2.java");        
        // templateConfig.setServiceImpl("templates/serviceimpl2.java");  
        templateConfig.setXml(null);  
        mpg.setTemplate(templateConfig);  
  
        // 策略配置  
        StrategyConfig strategy = new StrategyConfig();  
        strategy.setNaming(NamingStrategy.underline_to_camel);  
        strategy.setColumnNaming(NamingStrategy.underline_to_camel);  
		// strategy.setSuperEntityClass("你自己的父类实体,没有就不用设置!");  
		// strategy.setSuperEntityClass("BaseEntity");  
        strategy.setEntityLombokModel(true);  
        strategy.setRestControllerStyle(true);  
        // 公共父类  
		// strategy.setSuperControllerClass("BaseController");  
		// strategy.setSuperControllerClass("你自己的父类控制器,没有就不用设置!");  
        // 写于父类中的公共字段  
		// strategy.setSuperEntityColumns("id");  
        strategy.setInclude(scanner("表名，多个英文逗号分割").split(","));  
        strategy.setControllerMappingHyphenStyle(true);  
		// strategy.setTablePrefix(pc.getModuleName() + "_");  
        // 忽略表前缀tb_，例如：tb_user，直接映射为user对象  
        mpg.setStrategy(strategy);  
        mpg.setTemplateEngine(new FreemarkerTemplateEngine());  
        mpg.execute();  
    }  
}
```

## 注意点

这样生成的代码中对于`mapper`并没有自动注解`@Mapper`

解决办法：

- 1、添加 `Mapper` 注解

```java
package com.qtw.mapper;  
  
import com.qtw.entity.User;  
import com.baomidou.mybatisplus.core.mapper.BaseMapper;  
import org.apache.ibatis.annotations.Mapper;  
  
  
/**  
 * <p>  
 *  Mapper 接口  
 * </p>  
 *  
 * @author qtw  
 * @since 2022-12-22  
 */
@Mapper  // 添加注解
public interface UserMapper extends BaseMapper<User> {  
  
}
```


- 2、在启动类中，指定扫描`mapper`

```java
package com.qtw;  
  
import org.mybatis.spring.annotation.MapperScan;  
import org.springframework.boot.SpringApplication;  
import org.springframework.boot.autoconfigure.SpringBootApplication;  
  
@SpringBootApplication  
@MapperScan(value="com.qtw.mapper")   // 扫描 Mapperpublic 

class QtwApplication {    
	public static void main(String[] args) {  
	  SpringApplication.run(QtwApplication.class, args);  
	}  

}
```