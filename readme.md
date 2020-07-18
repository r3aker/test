##下面配置在nginx http 指令下添加（必须配置）
 lua_code_cache on;
 lua_shared_dict rate_limit 100m;
 lua_shared_dict rate_page_limit 100m;

##下面配置可以http 指令或者server指令下添加（可选配置）
 rewrite_by_lua_file  /usr/local/openresty/cc/real_ip.lua ;
 access_by_lua_file /usr/local/openresty/cc/rate_limit.lua;
 header_filter_by_lua_file /usr/local/openresty/cc/rate_page_limit.lua;

real_ip.lua: 获取用户真实IP并且通过X-Forwarded-For传递给后端。
rate_limit.lua: 限制请求频率，默认限制单IP1分钟请求500次，超过阀值返回403。
rate_page_limit：限制WEB页面请求频率，默认限制单IP1分钟请求200次，超过阀值返回403。

注意：如果access_by_lua_file或者header_filter_by_lua_file配置的话rewrite_by_lua_file必须配置。
 rewrite_by_lua_file可以单独配置。
