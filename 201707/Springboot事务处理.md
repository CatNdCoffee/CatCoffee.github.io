# SpringBoot与MyBatis事务

## 步骤

- 1.在mapper接口上添加@transactional注解
- 2.在需要的方法上添加@transacitonal注解
- 3.在application(main)类上添加@EnableTransactionManagement

## @transacitonal注解参数设置

@transactional(propagation = Propagation.REQUIRED, rollBackFor = Exception.class)