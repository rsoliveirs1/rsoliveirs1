#  Script SQL 

### RElatorio qt atendimento por tipo de clinica 

``` SQL
select 
b.DS_CLINICA 
       ,count(1) qtd_atend
       
      ,( select   count(1) 
       from ATENDIMENTO_PACIENTE_v ap 
       where ap.CD_CONVENIO =42 
       and ap.ie_tipo_atendimento = 3
        and obter_clinica_atend(ap.nr_atendimento,'') = b.DS_CLINICA 
        )QT_Atend_ps
      
       
       ,( select count(1)
          from procedimento_paciente x
          inner join procedimento p ON (p.cd_procedimento = x.cd_procedimento and p.ie_origem_proced = x.ie_origem_proced)
          where x.cd_convenio = 42
          and p.CD_SETOR_EXCLUSIVO  in (115,113,110,111,112)
          and obter_clinica_atend(x.nr_atendimento,'') = b.DS_CLINICA ) QT_EXAMES_CDI
        
        ,( select count(1)
          from procedimento_paciente x
          inner join procedimento p ON (p.cd_procedimento = x.cd_procedimento and p.ie_origem_proced = x.ie_origem_proced)
         where x.cd_convenio = 42
          and p.CD_SETOR_EXCLUSIVO  = 30
          and obter_clinica_atend(x.nr_atendimento,'') = b.DS_CLINICA ) QT_EXAMES_LAB , 
          
          ( select   count(1)
              from procedimento_paciente x
              inner join procedimento p ON (p.cd_procedimento = x.cd_procedimento and p.ie_origem_proced = x.ie_origem_proced)
              where x.cd_convenio = 42
              and CD_GRUPO_PROC in(30911,30912,83810)              
              and obter_clinica_atend(x.nr_atendimento,'') = b.DS_CLINICA
          )qt_proc_hemodinamica,
          
          ( select
                   count(1)
                from procedimento_paciente x
                inner join procedimento p ON (p.cd_procedimento = x.cd_procedimento and p.ie_origem_proced = x.ie_origem_proced)
                where  x.cd_convenio = 42
                and x.cd_setor_receita = 122          
                and obter_clinica_atend(x.nr_atendimento,'') = b.DS_CLINICA 
          )qt_proc_c_cirurgico
                      
          ,SUM(obter_valor_conta_atend(b.nr_atendimento)) VL_RECEITA
          
       
from atendimento_paciente_v b
where 1=1 
--and b.ie_tipo_atendimento = 1
and b.CD_CONVENIO = 42
group by --obter_desc_convenio(b.cd_convenio)---,
b.DS_CLINICA, b.ie_clinica
--obter_ds_setor_atendimento(b.CD_SETOR_ATENDIMENTO)
order by b.DS_CLINICA
;

````
