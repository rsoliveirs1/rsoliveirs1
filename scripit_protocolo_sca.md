
``` SQL
select 
pp.nr_atendimento,
pp.dt_prescricao dt_inclusão,
obter_nome_paciente(pp.nr_atendimento)nome_paciente,
obter_desc_mod_sae(pp.nr_seq_modelo)modelo_protocolo,
pe_obter_desc_diag(pd.nr_seq_diag,'DI')result_TIMI_RISK,
decode(
obter_desc_motivo_alta( aa.CD_MOTIVO_ALTA),
null,'Desfecho não registrado',
obter_desc_motivo_alta( aa.CD_MOTIVO_ALTA))tipo_alta,
aa.DT_DESFECHO

from pe_prescricao pp
left join ATENDIMENTO_ALTA aa on pp.nr_atendimento = aa.nr_atendimento
inner join PE_PRESCR_DIAG pd on pd.nr_seq_prescr = pp.nr_sequencia

where pp.NR_SEQ_MODELO in (13,14)
and pp.cd_pessoa_fisica not in (237142,229049)
order by obter_nome_paciente(pp.nr_atendimento),pp.dt_prescricao 
```
