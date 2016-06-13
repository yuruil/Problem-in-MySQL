# 终于解决了MYSQL的外码约束问题
* MYSQL查错可以用 SHOW ENGINE INNODB STATUS;命令，里面有一个LATEST FOREIGN KEY ERROR，显示了错误。
* 注意参照关系中的外码必须和被参照关系的主码的数据类型一致才行
* 检查引擎是否使用了INNODB，支持外码约束
* 可以使用set foreign_key_checks=0，解决被参照关系必须在参照关系前定义的问题
* 格式
* >CONSTRAINT constraint_name  
FOREIGN KEY （column_name1[, column_name2,…,column_name16]）                                           
REFERENCES ref_table [ （ref_column1[,ref_column2,…, ref_column16] ）]                                                              
[ ON DELETE { CASCADE | NO ACTION } ]                     
[ ON UPDATE { CASCADE | NO ACTION } ] ]                  
[ NOT FOR REPLICATION ]
* >create table course          					
	(course_id		varchar(8), 				
	 title			varchar(50), 				
	 dept_name		varchar(20),			
	 credits		numeric(2,0) check (credits > 0),
	 primary key (course_id),					
     constraint fk_course						
	 foreign key (dept_name) references department (dept_name)									
		on delete set null
	);
	
* 外关键字约束(Constraint...)定义了表之间的关系。当一个表中的一个列或多个列的组合和其它表中的主关键字定义相同时，就可以将这些列或列的组合定义为外关键字，并设定它适合哪个表中哪些列相关联。
* ON DELETE {CASCADE | NO ACTION}                  
指定在删除表中数据时，对关联表所做的相关操作。在子表中有数据行与父表中的对应数据行相关联的情况下，如果指定了值CASCADE，则在删除父表数据行时会将子表中对应的数据行删除；如果指定的是NO ACTION，则SQL Server 会产生一个错误，并将父表中的删除操作回滚。NO ACTION 是缺省值。 
* ON UPDATE {CASCADE | NO ACTION}
指定在更新表中数据时，对关联表所做的相关操作。在子表中有数据行与父表中的对应数据行相关联的情况下，如果指定了值CASCADE，则在更新父表数据行时会将子表中对应的数据行更新；如果指定的是NO ACTION，则SQL Server 会产生一个错误，并将父表中的更新操作回滚。NO ACTION 是缺省值。
* NOT FOR REPLICATION
指定列的外关键字约束在把从其它表中复制的数据插入到表中时不发生作用。