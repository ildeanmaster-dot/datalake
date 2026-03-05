SELECT
    sys_id                                  AS pi_nk,
    number                                  AS pi_number,
    short_description                       AS pi_nome,
    CAST(start_date AS DATE)                AS pi_inicio,
    CAST(end_date AS DATE)                  AS pi_fim,
    state                                   AS pi_state_code,
    dv_state                                AS pi_state_desc,
    active                                  AS pi_ativo,
    sn_safe_program                         AS trem_nk,
    dv_sn_safe_program                      AS trem_nome,
    sys_created_on                          AS criado_em,
    sys_updated_on                          AS atualizado_em
FROM dt0016_prd.servicenow_gdtec.vw_sn_safe_program_increment
WHERE sys_id IS NOT NULL
ORDER BY pi_number DESC
