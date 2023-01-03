project('wazuh', 'c')
dependencies = [
  dependency('libcjson'),
  dependency('zlib'),
  dependency('sqlite3'),
  dependency('openssl'),
  dependency('libplist-2.0'),
  dependency('libprocps'),
  #dependency('libdb'),
  dependency('libalpm'),
  dependency('libarchive'),
  dependency('yaml-0.1'),
  dependency('libcurl'),
  dependency('audit'),
  dependency('libffi'),
  dependency('msgpack'),
  dependency('bzip2'),
  dependency('libpcre2-8'),
  dependency('rpm'),
  dependency('jemalloc'),
  dependency('threads'),

]
default_cargs = ['-O3', '-DNDEBUG', '-DINOTIFY_ENABLED=1', '-D_XOPEN_SOURCE=600', '-D_GNU_SOURCE=1', '-DOSSECHIDS']
common_inc_dirs = include_directories(
  'headers', 
  'shared', 
  'wazuh_modules', 
  'wazuh_modules/syscollector/include',
  'shared_modules/common', 
  'data_provider/include')



config_o = static_library('config_o', [
'config/active-response.c',
'config/agentlessd-config.c',
'config/alerts-config.c',
'config/authd-config.c',
'config/authd-key-request-config.c',
'config/buffer-config.c',
'config/client-config.c',
'config/cluster-config.c',
'config/config.c',
'config/csyslogd-config.c',
'config/dbd-config.c',
'config/email-alerts-config.c',
'config/global-config.c',
'config/integrator-config.c',
'config/labels-config.c',
'config/localfile-config.c',
'config/logtest-config.c',
'config/remote-config.c',
'config/reports-config.c',
'config/rootcheck-config.c',
'config/rules-config.c',
'config/socket-config.c',
'config/syscheck-config.c',
'config/wazuh_db-config.c',
'config/wmodules-agent-upgrade.c',
'config/wmodules-aws.c',
'config/wmodules-azure.c',
'config/wmodules-ciscat.c',
'config/wmodules-command.c',
'config/wmodules-config.c',
'config/wmodules-docker.c',
'config/wmodules-fluent.c',
'config/wmodules-gcp.c',
'config/wmodules-github.c',
'config/wmodules-key-request.c',
'config/wmodules-office365.c',
'config/wmodules-oscap.c',
'config/wmodules-osquery-monitor.c',
'config/wmodules-sca.c',
'config/wmodules-syscollector.c',
'config/wmodules-task-manager.c',
'config/wmodules-vuln-detector.c',
], 
 c_args: default_cargs, dependencies: dependencies, include_directories: common_inc_dirs)

sql_schema_files = [
'wazuh_db/schema_agents.sql',
'wazuh_db/schema_global.sql',
'wazuh_db/schema_global_upgrade_v1.sql',
'wazuh_db/schema_global_upgrade_v2.sql',
'wazuh_db/schema_global_upgrade_v3.sql',
'wazuh_db/schema_global_upgrade_v4.sql',
'wazuh_db/schema_task_manager.sql',
'wazuh_db/schema_upgrade_v1.sql',
'wazuh_db/schema_upgrade_v2.sql',
'wazuh_db/schema_upgrade_v3.sql',
'wazuh_db/schema_upgrade_v4.sql',
'wazuh_db/schema_upgrade_v5.sql',
'wazuh_db/schema_upgrade_v6.sql',
'wazuh_db/schema_upgrade_v7.sql',
'wazuh_db/schema_upgrade_v8.sql',
'wazuh_db/schema_upgrade_v9.sql',
'wazuh_db/schema_vuln_detector.sql']

sql_sources = []
foreach file : sql_schema_files
  res = configure_file(input: file, output: '@BASENAME@.c', capture: true,
    command: ['echo', 'const char * @BASENAME@_sql = "' +run_command('cat', file, capture: true).stdout().replace('\n', '') + '";'])
  sql_sources += [ res ]
endforeach
wdblib_o = static_library('wdblib_o', [
'wazuh_db/wdb_agents.c',
'wazuh_db/wdb.c',
'wazuh_db/wdb_ciscat.c',
'wazuh_db/wdb_com.c',
'wazuh_db/wdb_delta_event.c',
'wazuh_db/wdb_fim.c',
'wazuh_db/wdb_global.c',
'wazuh_db/wdb_integrity.c',
'wazuh_db/wdb_metadata.c',
'wazuh_db/wdb_parser.c',
'wazuh_db/wdb_rootcheck.c',
'wazuh_db/wdb_sca.c',
'wazuh_db/wdb_scan_info.c',
'wazuh_db/wdb_state.c',
'wazuh_db/wdb_syscollector.c',
'wazuh_db/wdb_task.c',
'wazuh_db/wdb_upgrade.c',

'wazuh_db/helpers/wdb_agents_helpers.c',
'wazuh_db/helpers/wdb_global_helpers.c',


] + sql_sources, 
 c_args: default_cargs,  dependencies: dependencies, include_directories: common_inc_dirs)

wmodules_dep = static_library('wmodules_dep', [
  'wazuh_modules/wm_aws.c',
'wazuh_modules/wm_azure.c',
'wazuh_modules/wm_ciscat.c',
'wazuh_modules/wmcom.c',
'wazuh_modules/wm_command.c',
'wazuh_modules/wm_control.c',
'wazuh_modules/wm_database.c',
'wazuh_modules/wm_docker.c',
'wazuh_modules/wm_download.c',
'wazuh_modules/wm_exec.c',
'wazuh_modules/wm_fluent.c',
'wazuh_modules/wm_gcp.c',
'wazuh_modules/wm_github.c',
'wazuh_modules/wmodules.c',
'wazuh_modules/wm_office365.c',
'wazuh_modules/wm_oscap.c',
'wazuh_modules/wm_osquery_monitor.c',
'wazuh_modules/wm_sca.c',
'wazuh_modules/wm_syscollector.c',
'wazuh_modules/wm_task_general.c',

'wazuh_modules/agent_upgrade/wm_agent_upgrade.c',
# TODO: implement if for agent
'wazuh_modules/vulnerability_detector/wm_vuln_detector.c',
'wazuh_modules/vulnerability_detector/wm_vuln_detector_evr.c',
'wazuh_modules/vulnerability_detector/wm_vuln_detector_nvd.c',

'wazuh_modules/task_manager/wm_task_manager.c',
'wazuh_modules/task_manager/wm_task_manager_commands.c',
'wazuh_modules/task_manager/wm_task_manager_parsing.c',
'wazuh_modules/task_manager/wm_task_manager_tasks.c',

'wazuh_modules/agent_upgrade/manager/wm_agent_upgrade_commands.c',
'wazuh_modules/agent_upgrade/manager/wm_agent_upgrade_manager.c',
'wazuh_modules/agent_upgrade/manager/wm_agent_upgrade_parsing.c',
'wazuh_modules/agent_upgrade/manager/wm_agent_upgrade_tasks.c',
'wazuh_modules/agent_upgrade/manager/wm_agent_upgrade_tasks_callbacks.c',
'wazuh_modules/agent_upgrade/manager/wm_agent_upgrade_upgrades.c',
'wazuh_modules/agent_upgrade/manager/wm_agent_upgrade_validate.c',

], 
 c_args: default_cargs, dependencies: dependencies, include_directories: common_inc_dirs, link_with: wdblib_o)

crypto_o = static_library('crypto_o', [
'os_crypto/aes/aes_op.c',
'os_crypto/blowfish/bf_op.c',
'os_crypto/hmac/hmac.c',
'os_crypto/md5/md5_op.c',
'os_crypto/md5_sha1/md5_sha1_op.c',
'os_crypto/md5_sha1_sha256/md5_sha1_sha256_op.c',
'os_crypto/sha1/sha1_op.c',
'os_crypto/sha256/sha256_op.c',
'os_crypto/sha512/sha512_op.c',
'os_crypto/shared/keys.c',
'os_crypto/shared/msgs.c',
'os_crypto/signature/signature.c',
], 
 c_args: default_cargs, dependencies: dependencies, include_directories: common_inc_dirs)


shared_o = static_library('shared_o', [
'shared/agent_op.c',
'shared/atomic.c',
'shared/audit_op.c',
'shared/auth_client.c',
'shared/b64.c',
'shared/bqueue_op.c',
'shared/buffer_op.c',
'shared/bzip2_op.c',
'shared/cluster_utils.c',
'shared/custom_output_search_replace.c',
'shared/debug_op.c',
'shared/enrollment_op.c',
'shared/exec_op.c',
'shared/expression.c',
'shared/file_op.c',
'shared/file-queue.c',
'shared/fs_op.c',
'shared/hash_op.c',
'shared/help.c',
'shared/integrity_op.c',
'shared/json_op.c',
'shared/json-queue.c',
'shared/labels_op.c',
'shared/list_op.c',
'shared/log_builder.c',
'shared/math_op.c',
'shared/mem_op.c',
'shared/mq_op.c',
'shared/notify_op.c',
'shared/os_utils.c',
'shared/privsep_op.c',
'shared/pthreads_op.c',
'shared/queue_linked_op.c',
'shared/queue_op.c',
'shared/randombytes.c',
'shared/rbtree_op.c',
'shared/read-agents.c',
'shared/read-alert.c',
'shared/regex_op.c',
'shared/remoted_op.c',
'shared/report_op.c',
'shared/request_op.c',
'shared/rootcheck_op.c',
'shared/rules_op.c',
'shared/rwlock_op.c',
'shared/schedule_scan.c',
'shared/sig_op.c',
'shared/store_op.c',
'shared/string_op.c',
'shared/sym_load.c',
'shared/syscheck_op.c',
'shared/sysinfo_utils.c',
'shared/time_op.c',
'shared/url.c',
'shared/utf8_op.c',
'shared/validate_op.c',
'shared/vector_op.c',
'shared/version_op.c',
'shared/wait_op.c',
'shared/wazuhdb_op.c',
'shared/yaml2json.c',
], 
 c_args: [default_cargs, '-fPIC', '-DARGV0="wazuh-remoted"'], dependencies: dependencies , include_directories: common_inc_dirs)

os_net_o = static_library('os_net_o', [
  'os_net/os_net.c',
], 
 c_args: [default_cargs, '-fPIC'], dependencies: dependencies, include_directories: common_inc_dirs)

os_regex_o = static_library('os_regex_o', [
  'os_regex/os_match.c',
'os_regex/os_match_compile.c',
'os_regex/os_match_execute.c',
'os_regex/os_match_free_pattern.c',
'os_regex/os_regex.c',
'os_regex/os_regex_compile.c',
'os_regex/os_regex_execute.c',
'os_regex/os_regex_free_pattern.c',
'os_regex/os_regex_maps.c',
'os_regex/os_regex_match.c',
'os_regex/os_regex_startswith.c',
'os_regex/os_regex_strbreak.c',
'os_regex/os_regex_str.c',
], 
 c_args: [default_cargs, '-fPIC'], dependencies: dependencies, include_directories: common_inc_dirs)

os_xml_o = static_library('os_xml_o', [
'os_xml/os_xml_access.c',
'os_xml/os_xml.c',
'os_xml/os_xml_node_access.c',
'os_xml/os_xml_variables.c',
'os_xml/os_xml_writer.c',
], 
 c_args: [default_cargs, '-fPIC'], dependencies: dependencies, include_directories: common_inc_dirs)

os_zlib_o = static_library('os_zlib_o', [
  'os_zlib/os_zlib.c',
], 
 c_args: [default_cargs, '-fPIC'], dependencies: dependencies, include_directories: common_inc_dirs)
libwazuh_a = static_library('wazuh', [
    'os_auth/ssl.c',
    'os_auth/check_cert.c',
    'addagent/validate.c',
    'analysisd/logmsg.c'
  ],
  link_with: [
    config_o, 
    wmodules_dep,
    crypto_o,
    shared_o,
    os_net_o,
    os_regex_o,
    os_xml_o,
    os_zlib_o,
  ], c_args: [default_cargs], dependencies: dependencies, include_directories: common_inc_dirs)



alerts_a = static_library('alerts', [
'analysisd/alerts/exec.c',
'analysisd/alerts/getloglocation.c',
'analysisd/alerts/log.c',
], c_args: default_cargs, dependencies: dependencies, include_directories: [common_inc_dirs, include_directories('analysisd', 'analysisd/cdb')])

decoders_live_a = static_library('decoders_live', [
'analysisd/decoders/ciscat.c',
'analysisd/decoders/dbsync.c',
'analysisd/decoders/decoder.c',
'analysisd/decoders/decoders_list.c',
'analysisd/decoders/decode-xml.c',
'analysisd/decoders/geoip.c',
'analysisd/decoders/hostinfo.c',
'analysisd/decoders/plugin_decoders.c',
'analysisd/decoders/rootcheck.c',
'analysisd/decoders/security_configuration_assessment.c',
'analysisd/decoders/syscheck.c',
'analysisd/decoders/syscollector.c',
'analysisd/decoders/winevtchannel.c',

'analysisd/decoders/plugins/json_decoder.c',
'analysisd/decoders/plugins/ossecalert_decoder.c',
'analysisd/decoders/plugins/pf_decoder.c',
'analysisd/decoders/plugins/sonicwall_decoder.c',
'analysisd/decoders/plugins/symantecws_decoder.c',

'analysisd/compiled_rules/generic_samples.c',

], c_args: [default_cargs, '-DARGV0="wazuh-analysisd"'], dependencies: dependencies, include_directories: [common_inc_dirs, include_directories('analysisd', 'analysisd/cdb')])

decoders_test_a = static_library('decoders_test', [
'analysisd/decoders/ciscat.c',
'analysisd/decoders/dbsync.c',
'analysisd/decoders/decoder.c',
'analysisd/decoders/decoders_list.c',
'analysisd/decoders/decode-xml.c',
'analysisd/decoders/geoip.c',
'analysisd/decoders/hostinfo.c',
'analysisd/decoders/plugin_decoders.c',
'analysisd/decoders/rootcheck.c',
'analysisd/decoders/security_configuration_assessment.c',
'analysisd/decoders/syscheck.c',
'analysisd/decoders/syscollector.c',
'analysisd/decoders/winevtchannel.c',

'analysisd/decoders/plugins/json_decoder.c',
'analysisd/decoders/plugins/ossecalert_decoder.c',
'analysisd/decoders/plugins/pf_decoder.c',
'analysisd/decoders/plugins/sonicwall_decoder.c',
'analysisd/decoders/plugins/symantecws_decoder.c',

'analysisd/compiled_rules/generic_samples.c',

], c_args: [default_cargs, '-DTESTRULE', '-DARGV0="wazuh-analysisd"'], dependencies: dependencies, include_directories: [common_inc_dirs, include_directories('analysisd', 'analysisd/cdb')])

format_o = static_library('format', [
'analysisd/format/json_extended.c',
'analysisd/format/to_json.c',
], c_args: default_cargs, dependencies: dependencies, include_directories: [common_inc_dirs, include_directories('analysisd', 'analysisd/cdb')])

output_o = static_library('output', [
'analysisd/output/jsonout.c',
'analysisd/output/prelude.c',
'analysisd/output/zeromq.c',
], c_args: default_cargs, dependencies: dependencies, include_directories: [common_inc_dirs, include_directories('analysisd', 'analysisd/cdb')])

cdb_a = static_library('cdb', [
'analysisd/cdb/cdb.c',
'analysisd/cdb/cdb_hash.c',
'analysisd/cdb/cdb_make.c',
'analysisd/cdb/uint32_pack.c',
'analysisd/cdb/uint32_unpack.c',
], c_args: default_cargs, dependencies: dependencies, include_directories: [common_inc_dirs, include_directories('analysisd', 'analysisd/cdb')])

configure_file(output: 'dummy', command: ['analysisd/compiled_rules/register_rule.sh', 'build'])

analysisd_o = static_library('analysisd', [
  'analysisd/accumulator.c',
'analysisd/active-response.c',
'analysisd/ar_json.c',
'analysisd/asyscom.c',
'analysisd/cleanevent.c',
'analysisd/config.c',
'analysisd/config_json.c',
'analysisd/dodiff.c',
'analysisd/eventinfo.c',
'analysisd/eventinfo_list.c',
'analysisd/fts.c',
'analysisd/labels.c',
'analysisd/limits.c',
'analysisd/lists.c',
'analysisd/lists_list.c',
'analysisd/lists_make.c',
'analysisd/logtest.c',
'analysisd/mitre.c',
'analysisd/rules.c',
'analysisd/rules_list.c',
'analysisd/state.c',
'analysisd/stats.c',
  ], c_args: [default_cargs], dependencies: dependencies, include_directories: [common_inc_dirs, include_directories('analysisd', 'analysisd/cdb')])




analysisd_testrule_test = static_library('analysisd_testrule_test', [
  'analysisd/eventinfo.c',
  'analysisd/testrule.c',
  'analysisd/analysisd.c'
  ], c_args: [default_cargs, '-DTESTRULE'], dependencies: dependencies, include_directories: [include_directories('analysisd', 'analysisd/cdb'), common_inc_dirs])

analysisd_testrule_live = static_library('analysisd_testrule_live', [
  'analysisd/eventinfo.c',
  'analysisd/analysisd.c'
  ], c_args: [default_cargs, '-DARGV0="wazuh-analysisd"'], dependencies: dependencies, include_directories: [include_directories('analysisd', 'analysisd/cdb'), common_inc_dirs])

rootcheck_a = static_library('rootcheck', [
'rootcheck/check_open_ports.c',
'rootcheck/check_rc_dev.c',
'rootcheck/check_rc_files.c',
'rootcheck/check_rc_if.c',
'rootcheck/check_rc_pids.c',
'rootcheck/check_rc_policy.c',
'rootcheck/check_rc_ports.c',
'rootcheck/check_rc_readproc.c',
'rootcheck/check_rc_sys.c',
'rootcheck/check_rc_trojans.c',
'rootcheck/common.c',
'rootcheck/common_rcl.c',
'rootcheck/config.c',
'rootcheck/os_string.c',
'rootcheck/rootcheck.c',
'rootcheck/run_rk_check.c',
'rootcheck/unix-process.c',
'rootcheck/win-common.c',
'rootcheck/win-process.c',

  ], c_args: [default_cargs, '-DARGV0="rootcheck"'], dependencies: dependencies, include_directories: common_inc_dirs)

schema_fim_db = configure_file(input: 'syscheckd/db/schema_fim_db.sql', output: '@BASENAME@.c', capture: true,
    command: ['echo', 'const char * schema_fim_sql = "' +run_command('cat', 'syscheckd/db/schema_fim_db.sql', capture: true).stdout().replace('\n', '') + '";'])

syschecklib = static_library('syschecklib', [
'syscheckd/config.c',
'syscheckd/create_db.c',
'syscheckd/fim_diff_changes.c',
'syscheckd/fim_sync.c',
'syscheckd/main.c',
'syscheckd/run_check.c',
'syscheckd/run_realtime.c',
'syscheckd/syscheck.c',
'syscheckd/syscom.c',

'syscheckd/db/fim_db.c',
'syscheckd/db/fim_db_files.c',
'syscheckd/db/fim_db_registries.c',

'syscheckd/whodata/audit_healthcheck.c',
'syscheckd/whodata/audit_parse.c',
'syscheckd/whodata/audit_rule_handling.c',
'syscheckd/whodata/syscheck_audit.c',
'syscheckd/whodata/win_whodata.c',

'syscheckd/registry/events.c',
'syscheckd/registry/registry.c',

schema_fim_db

  ], c_args: [default_cargs, '-DARGV0="wazuh-syscheckd"'], dependencies: dependencies, include_directories: common_inc_dirs)

syscheck_eventchannel_o = static_library('syscheck_eventchannel_o', [
'syscheckd/config.c',
'syscheckd/create_db.c',
'syscheckd/fim_diff_changes.c',
'syscheckd/fim_sync.c',
'syscheckd/main.c',
'syscheckd/run_check.c',
'syscheckd/run_realtime.c',
'syscheckd/syscheck.c',
'syscheckd/syscom.c',

'syscheckd/db/fim_db.c',
'syscheckd/db/fim_db_files.c',
'syscheckd/db/fim_db_registries.c',

'syscheckd/whodata/audit_healthcheck.c',
'syscheckd/whodata/audit_parse.c',
'syscheckd/whodata/audit_rule_handling.c',
'syscheckd/whodata/syscheck_audit.c',
'syscheckd/whodata/win_whodata.c',

'syscheckd/registry/events.c',
'syscheckd/registry/registry.c',

schema_fim_db
  ], c_args: [default_cargs, '-DEVENTCHANNEL_SUPPORT', '-DARGV0="wazuh-syscheckd"'], dependencies: dependencies, include_directories: common_inc_dirs)

active_response_o = static_library('active_response', [
  'active-response/active_responses.c',
'active-response/disable-account.c',
'active-response/firewalld-drop.c',
'active-response/host-deny.c',
'active-response/ip-customblock.c',
'active-response/kaspersky.c',
'active-response/netsh.c',
'active-response/restart-wazuh.c',
'active-response/route-null.c',
'active-response/wazuh-slack.c',
  ], c_args: [default_cargs, '-DARGV0="active-responses"' ], dependencies: dependencies, include_directories: [common_inc_dirs, 'active-response'])

executable('wazuh-maild', [
'os_maild/config.c',
'os_maild/mailcom.c',
'os_maild/maild.c',
'os_maild/mail_list.c',
'os_maild/os_maild_client.c',
'os_maild/sendcustomemail.c',
'os_maild/sendmail.c',
  ], install: true, c_args: [default_cargs, '-DARGV0="wazuh-maild"'], dependencies: dependencies, include_directories: common_inc_dirs, link_with: [libwazuh_a])

executable('wazuh-csyslogd', [
'os_csyslogd/alert.c',
'os_csyslogd/config.c',
'os_csyslogd/csyscom.c',
'os_csyslogd/csyslogd.c',
'os_csyslogd/main.c',

  ], install: true, c_args: [default_cargs, '-DARGV0="wazuh-csyslogd"'], dependencies: dependencies, include_directories: common_inc_dirs, link_with: [libwazuh_a])

executable('wazuh-agentlessd', [
'agentlessd/agentlessd.c',
'agentlessd/lessdcom.c',
'agentlessd/main.c',
  ], install: true, c_args: [default_cargs, '-DARGV0="wazuh-agentlessd"'], dependencies: dependencies, include_directories: common_inc_dirs, link_with: [libwazuh_a])

executable('wazuh-execd', [
'os_execd/config.c',
'os_execd/exec.c',
'os_execd/execd.c',
'os_execd/main.c',
'os_execd/wcom.c',

'active-response/active_responses.c'
  ], install: true, c_args: [default_cargs, '-DARGV0="wazuh-execd"'], dependencies: dependencies, include_directories: common_inc_dirs, link_with: [libwazuh_a])


executable('wazuh-logcollector', [
  'logcollector/config.c',
'logcollector/lccom.c',
'logcollector/logcollector.c',
'logcollector/macos_log.c',
'logcollector/main.c',
'logcollector/read_audit.c',
'logcollector/read_command.c',
'logcollector/read_djb_multilog.c',
'logcollector/read_fullcommand.c',
'logcollector/read_json.c',
'logcollector/read_macos.c',
'logcollector/read_mssql_log.c',
'logcollector/read_multiline.c',
'logcollector/read_multiline_regex.c',
'logcollector/read_mysql_log.c',
'logcollector/read_nmapg.c',
'logcollector/read_ossecalert.c',
'logcollector/read_postgresql_log.c',
'logcollector/read_snortfull.c',
'logcollector/read_syslog.c',
'logcollector/read_ucs2_be.c',
'logcollector/read_ucs2_le.c',
'logcollector/read_win_el.c',
'logcollector/read_win_event_channel.c',
'logcollector/state.c',
  ], install: true, c_args: [default_cargs, '-DARGV0="wazuh-logcollector"'], dependencies: dependencies, include_directories: common_inc_dirs, link_with: [libwazuh_a])

executable('wazuh-remoted', [
  'remoted/ar-forward.c',
'remoted/cfga-forward.c',
'remoted/config.c',
'remoted/main.c',
'remoted/manager.c',
'remoted/netbuffer.c',
'remoted/netcounter.c',
'remoted/queue.c',
'remoted/remcom.c',
'remoted/remoted.c',
'remoted/request.c',
'remoted/secure.c',
'remoted/sendmsg.c',
'remoted/shared_download.c',
'remoted/state.c',
'remoted/syslog.c',
'remoted/syslogtcp.c',
  ], install: true, c_args: [default_cargs, '-DARGV0="wazuh-remoted"'], dependencies: dependencies, include_directories: common_inc_dirs, link_with: [libwazuh_a])

executable('wazuh-agentd', [
  'client-agent/agcom.c',
'client-agent/agentd.c',
'client-agent/buffer.c',
'client-agent/config.c',
'client-agent/event-forward.c',
'client-agent/main.c',
'client-agent/notify.c',
'client-agent/receiver.c',
'client-agent/request.c',
'client-agent/restart_agent.c',
'client-agent/rotate_log.c',
'client-agent/sendmsg.c',
'client-agent/start_agent.c',
'client-agent/state.c',

'monitord/rotate_log.c',
'monitord/compress_log.c',
  ], install: true, c_args: [default_cargs, '-DARGV0="wazuh-agentd"'], dependencies: dependencies, include_directories: common_inc_dirs, link_with: [libwazuh_a])

executable('manage_agents', [
'addagent/main.c',
'addagent/manage_agents.c',
'addagent/manage_keys.c',
'addagent/read_from_user.c',
'addagent/validate.c',
  ], install: true, c_args: [default_cargs, '-DARGV0="manage_agents"'], dependencies: dependencies, include_directories: common_inc_dirs, link_with: [libwazuh_a])

executable('clear_stats', [
  'util/clear_stats.c'
  ], install: true, c_args: default_cargs, dependencies: dependencies, include_directories: common_inc_dirs, link_with: [libwazuh_a])

executable('verify-agent-conf', [
  'util/verify-agent-conf.c'
  ], install: true, c_args: default_cargs, dependencies: dependencies, include_directories: common_inc_dirs, link_with: [libwazuh_a])

executable('agent_control', [
  'util/agent_control.c',
  'addagent/validate.c'
  ], install: true, c_args: default_cargs, dependencies: dependencies, include_directories: common_inc_dirs, link_with: [libwazuh_a])

executable('wazuh-regex', [
  'util/wazuh-regex.c'
  ], install: true, c_args: default_cargs, dependencies: dependencies, include_directories: common_inc_dirs, link_with: [libwazuh_a])

executable('parallel-regex', [
  'util/parallel-regex.c'
  ], install: true, c_args: default_cargs, dependencies: dependencies, include_directories: common_inc_dirs, link_with: [libwazuh_a])

executable('default-firewall-drop', [
  'active-response/firewalls/default-firewall-drop.c', 'active-response/active_responses.c'
  ], install: true, c_args: default_cargs, dependencies: dependencies, include_directories: common_inc_dirs, link_with: [libwazuh_a])

executable('pf', [
  'active-response/firewalls/pf.c', 'active-response/active_responses.c'
  ], install: true, c_args: default_cargs, dependencies: dependencies, include_directories: common_inc_dirs, link_with: [libwazuh_a])

executable('npf', [
  'active-response/firewalls/npf.c', 'active-response/active_responses.c'
  ], install: true, c_args: default_cargs, dependencies: dependencies, include_directories: common_inc_dirs, link_with: [libwazuh_a])

executable('ipfw', [
  'active-response/firewalls/ipfw.c', 'active-response/active_responses.c'
  ], install: true, c_args: default_cargs, dependencies: dependencies, include_directories: common_inc_dirs, link_with: [libwazuh_a])

executable('firewalld-drop', [
  'active-response/firewalld-drop.c', 'active-response/active_responses.c'
  ], install: true, c_args: default_cargs, dependencies: dependencies, include_directories: common_inc_dirs, link_with: [libwazuh_a])

executable('disable-account', [
  'active-response/disable-account.c', 'active-response/active_responses.c'
  ], install: true, c_args: default_cargs, dependencies: dependencies, include_directories: common_inc_dirs, link_with: [libwazuh_a])

executable('host-deny', [
  'active-response/host-deny.c', 'active-response/active_responses.c'
  ], install: true, c_args: default_cargs, dependencies: dependencies, include_directories: common_inc_dirs, link_with: [libwazuh_a])

executable('ip-customblock', [
  'active-response/ip-customblock.c', 'active-response/active_responses.c'
  ], install: true, c_args: default_cargs, dependencies: dependencies, include_directories: common_inc_dirs, link_with: [libwazuh_a])

executable('restart-wazuh', [
  'active-response/restart-wazuh.c', 'active-response/active_responses.c'
  ], install: true, c_args: default_cargs, dependencies: dependencies, include_directories: common_inc_dirs, link_with: [libwazuh_a])

executable('route-null', [
  'active-response/route-null.c', 'active-response/active_responses.c'
  ], install: true, c_args: default_cargs, dependencies: dependencies, include_directories: common_inc_dirs, link_with: [libwazuh_a])

executable('kaspersky', [
  'active-response/kaspersky.c', 'active-response/active_responses.c'
  ], install: true, c_args: default_cargs, dependencies: dependencies, include_directories: common_inc_dirs, link_with: [libwazuh_a])

executable('wazuh-slack', [
  'active-response/wazuh-slack.c', 'active-response/active_responses.c'
  ], install: true, c_args: default_cargs, dependencies: dependencies, include_directories: common_inc_dirs, link_with: [libwazuh_a])

executable('wazuh-syscheckd', [
  ], install: true, c_args: [default_cargs, '-DARGV0="wazuh-reportd"' ], dependencies: dependencies, include_directories: common_inc_dirs, link_with: [libwazuh_a, syschecklib, rootcheck_a])

executable('wazuh-monitord', [
'monitord/compress_log.c',
'monitord/generate_reports.c',
'monitord/main.c',
'monitord/manage_files.c',
'monitord/moncom.c',
'monitord/monitor_actions.c',
'monitord/monitord.c',
'monitord/rotate_log.c',
'monitord/sign_log.c',

'os_maild/sendcustomemail.c'
  ], install: true, c_args: [default_cargs, '-DARGV0="wazuh-monitord"' ], dependencies: dependencies, include_directories: common_inc_dirs, link_with: [libwazuh_a])

executable('wazuh-reportd', [
'reportd/report.c',
  ], install: true, c_args: [default_cargs, '-DARGV0="wazuh-reportd"' ], dependencies: dependencies, include_directories: common_inc_dirs, link_with: [libwazuh_a])

executable('agent-auth', [
  'addagent/validate.c',
  'os_auth/main-client.c',
  'os_auth/check_cert.c',
  'os_auth/ssl.c',
  ], install: true, c_args: [default_cargs, '-DARGV0="wazuh-authd"' ], dependencies: dependencies, include_directories: common_inc_dirs, link_with: [libwazuh_a])

executable('wazuh-authd', [
  'addagent/validate.c',
  'os_auth/main-server.c',
  'os_auth/local-server.c',
  'os_auth/check_cert.c',
  'os_auth/config.c',
  'os_auth/authcom.c',
  'os_auth/auth.c',
  'os_auth/key_request.c',
  'os_auth/generate_cert.c',
  ], install: true, c_args: [default_cargs, '-DARGV0="wazuh-authd"' ], dependencies: dependencies, include_directories: common_inc_dirs, link_with: [libwazuh_a])



executable('wazuh-logtest-legacy', [
  ], install: true, c_args: [default_cargs, '-DARGV0="wazuh-logtest-legacy"' ], dependencies: dependencies, include_directories: common_inc_dirs, link_with: [libwazuh_a, analysisd_o, output_o, format_o, alerts_a, cdb_a, decoders_test_a, analysisd_testrule_test])

executable('wazuh-analysisd', [
  'analysisd/logmsg.c'
  ], install: true, c_args: [default_cargs, '-DARGV0="wazuh-analysisd"' ], dependencies: dependencies, include_directories: common_inc_dirs, link_with: [libwazuh_a, analysisd_o, output_o, format_o, alerts_a, cdb_a, decoders_live_a, analysisd_testrule_live])


executable('wazuh-dbd', [
  'os_dbd/alert.c',
'os_dbd/config.c',
'os_dbd/dbd.c',
'os_dbd/db_op.c',
'os_dbd/main.c',
'os_dbd/rules.c',
'os_dbd/server.c',
  ], install: true, c_args: [default_cargs, '-DARGV0="wazuh-dbd"' ], dependencies: dependencies, include_directories: common_inc_dirs, link_with: [libwazuh_a])

executable('wazuh-integratord', [
'os_integrator/config.c',
'os_integrator/integrator.c',
'os_integrator/intgcom.c',
'os_integrator/main.c',
  ], install: true, c_args: [default_cargs, '-DARGV0="wazuh-integratord"' ], dependencies: dependencies, include_directories: common_inc_dirs, link_with: [libwazuh_a])

executable('wazuh-modulesd', ['wazuh_modules/main.c'], install: true, c_args: [default_cargs, '-DARGV0="wazuh-modulesd"' ], dependencies: dependencies, include_directories: common_inc_dirs, link_with: [libwazuh_a])

executable('wazuh-db', ['wazuh_db/main.c'], install: true, c_args: [default_cargs, '-DARGV0="wazuh-db"' ], dependencies: dependencies, include_directories: common_inc_dirs, link_with: [wdblib_o, libwazuh_a])
