	
基因https://github.com/Eonblast/Emysql修复编译bug      
(1） Compiling _build/default/lib/emysql/src/emysql_app.erl failed _build/default/lib/emysql/include/emysql.hrl:38: type gb_tree() undefined _build/default/lib/emysql/include/emysql.hrl:39: type queue() undefined
      解决：
      		找到emysql/include/emysql.hrl 38：修改如下
	       available=queue:new() ,%:: queue(), 
	       locked=gb_trees:empty() ,%:: gb_tree(), 
	       waiting=queue:new() ,%:: queue(), 

(2)emysql/src/emysql_conn_mgr.erl:43: type dict() undefined
	解决如下：
		%-record(state, {pools, lockers = dict:new() :: dict()}).
		
		-record(state, {pools, lockers = dict:new()}).

(3)emysql/src/emysql.erl:672: type dict() undefined
	解决如下：
	
% -spec as_dict(Result) -> Dict
%   when
%     Result :: #result_packet{},
%     Dict :: dict().

as_dict(Res) -> emysql_conv:as_dict(Res).

(4) 两个关于时间的warming,原因新的otp已经换了时间函数
emysql/src/emysql_conn_mgr.erl:422: Warning: erlang:now/0: Deprecated BIF. See the "Time and Time Correction in Erlang" chapter of the ERTS User's Guide for more information.
/emysql/src/emysql_conn.erl:337: Warning: erlang:now/0: Deprecated BIF. See the "Time and Time Correction in Erlang" chapter of the ERTS User's Guide for more information.

erlang:now 改 erlang:timestamp()



忽略：
no changes needed to "include/crypto_compat.hrl". Skipping writing new file
