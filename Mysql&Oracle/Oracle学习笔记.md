# 创建表
```sql
create table test (names varchar2(12),
                   dates date,
                   num   int,
                   dou   double);
```

# 创建或删除视图
## 创建视图
```sql
create or replace view vcregion as
select d.RegionName as DistrictName,
       c.RegionName as StreetName,
       b.RegionName as CommunityName,
       a.RegionName as CellName,
       d.RegionID   as DistrictID,
       c.RegionID   as StreetID,
       b.RegionID   as CommunityID,
       a.RegionID   as CellID,
       SubStr(c.JDDM,1,6) as CQDM,
       c.JDDM,b.SQDM,a.WGDM
  from dlsys.tcRegion a,
       dlsys.tcRegion b,
       dlsys.tcRegion c,
       dlsys.tcRegion d
  where a.RegionType = 5
    and a.SeniorID = b.RegionID
    and b.SeniorID = c.RegionID
    and c.SeniorID = d.RegionID;
```
## 删除视图
```sql
drop view 视图名 
```

# 同义词
```sql
create or replace synonym aa
for dbusrcard001.aa;
```

# 存储过程
```sql
create or replace procedure dlmis.poPatrolGuardCheck(
) is
/**
功能：
类别：工具/迁移/基础/普通
参数：
返回值：
历史：
2017-11-10 xxx 初稿
**/
  iPatrolid        integer;
  sPatrolname      varchar2(40);
  iLogid           integer;
  iRouteid         integer;
   iCoordNum         integer;
  fPlanX           float;
  fPlanY           float;
  fCoordX          float;
  fCoordY          float;
  curPatrolSet     sys_refcursor;
  curPointSet      sys_refcursor;
  iCheckFlag       integer :=0;
  fDistance        number;
  dUpdatetime      date;
begin
    --获取监督员的所有巡更点
    open curPatrolSet for select a.patrolid,c.patrolname,b.routeid,b.coordinatex,b.coordinatey from dlsys.tcpatrolroute a,dlsys.tcroutepoint b,dlsys.tcpatrol c
    where a.routeid = b.routeid and a.patrolid = c.patrolid;
    loop
       fetch curPatrolSet into iPatrolid,sPatrolname,iRouteid,fPlanX,fPlanY;
       exit when curPatrolSet%notfound;
       
        --如果当天定位数为0,不检测 add by dyx 2017-11-06
       select count(*) into iCoordNum from dlmis.trLogPatrolPos
       where patrolid = iPatrolid and updatetime >= trunc(sysdate) and updatetime < trunc(sysdate)+1;
       if iCoordNum=0 then
         continue;
       end if;
       
       --针对巡更点，匹配轨迹
       iCheckFlag := 0;
       open curPointSet for select logid,coordinatex,coordinatey,updatetime from dlmis.trLogPatrolPos
       where patrolid = iPatrolid and updatetime >= trunc (sysdate) and updatetime < trunc(sysdate)+1;
       loop
         fetch curPointSet into iLogid,fCoordX,fCoordY,dUpdatetime;
         exit when curPointSet%notfound;
         fDistance := sqrt(power((fPlanX-fCoordX),2)+power((fPlanY-fCoordY),2));
         if fDistance <=200 then
           iCheckFlag := 1;
           exit;
         end if;
       end loop;
       select decode(dUpdatetime,null,sysdate,dUpdatetime) into dUpdatetime from dual;
       insert into dlmis.toPatrolGuardCheck(patrolid,patrolname,logid,routeid,coordX,coordY,checkdate,checkflag)
       values(iPatrolid,sPatrolname,iLogid,iRouteid,fPlanX,fPlanY,dUpdatetime,iCheckFlag);
    end loop;
    commit;
exception
  when others then
    rollback;
    plog.error('监督员巡更点检查失败{时间=' ||to_char(sysdate,'yyyy-mm-dd hh24:mi:ss') || '}');
end poPatrolGuardCheck;
```

# 创建函数
```sql

create or replace function ee(v_id in employee%rowtype) return varchar(15)
is
var_test varchar2(15);
begin
  return var_test;
exception when others then
   
end
```

# 三种触发器的定义
```sql
-- 第一种
create or replace trigger ff
alter delete
on test
for each row
declare
begin
   delete from test;
   if sql%rowcount < 0 or sql%rowcount is null then
      rais_replaction_err(-20004,"错误")
   end if
end
-- 第二种
create or replace trigger gg
alter insert
on test
for each row
declare
begin
   if :old.names = :new.names then
      raise_replaction_err(-2003,"编码重复");
   end if
end
-- 第三种
create or replace trigger hh
for update
on test
for each row
declare
begin
  if updating then
     if :old.names <> :new.names then
 reaise_replaction_err(-2002,"关键字不能修改")
     end if
  end if
end
```

# 定义游标
```sql
declare
   cursor aa is
      select names,num from test;
begin
   for bb in aa
   loop
        if bb.names = "ORACLE" then
        
        end if
   end loop;
end
```

## 通过字段类型查询字段

```sql
SELECT b.column_name column_name --字段名
      ,b.data_type data_type     --字段类型
      ,b.data_length             --字段长度
      ,a.comments comments       --字段注释
      ,A.table_name table_name    --表名
FROM user_col_comments a
    ,all_tab_columns b
WHERE a.table_name = b.table_name and
    b.data_type = 'BLOB' 
    group by b.column_name,b.data_type,b.DATA_LENGTH,a.comments,
    a.table_name;
```

