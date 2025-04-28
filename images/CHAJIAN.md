#### 静态插件（全局、市场插件）

```tex
SysPlugin {
	ID 				string	
	Name			string	"不可重复"
	AliasName		string	"显示名"
	Category		string	"类型，枚举：一般类型|初始化类型|入口网络|出口网络|出口入口共治网络|性能分析|监控"
	Image			string	"镜像名"
	Command			string	"执行命令，可为空；多为初始化容器时使用。"
	CPU				string	"例如：100m"
	Memory			string	"例如：128M" 
	Desc			string	"描述信息"
	ConfigGroups	[]ConfigGroup
}
PluginConfig => 'plugin_config'{
	ID				string
	Name			string	"配置组名称"
	ServiceMetaType	string	"un_define|upstream_port|downstream_port"
	Injection		string	"env|auto"
	Options			[]PluginConfigOption
	PluginID		string
}
PluginConfigOption => 'plugin_config_ption'{
	AttrName			string	"配置名"
	AttrType			string	"string|radio|checkbox"
	AttrDefaultValue	string	"配置默认值"
	IsChange			bool	"组件开通插件后，插件配置项的值是否可以修改"
	AttrAltValue		string	"如果AttrType为radio或checkbox时，radio或checkbox的列表值，例如：RED,GREEN,GRAY"
	AttrInfo			string	"配置项的描述"
	Protocol			string	"协议"
	PluginConfigID		string
}
```

#### 导入导出插件

插件导入导出json结构

```json
{
  "perf_analyze_plugin": {
    "plugin_alias": "服务实时性能分析",
    "desc": "实时分析应用的吞吐率、响应时间、在线人数等指标",
    "category": "analyst-plugin:perf",
    "code_repo": "",
    "build_source": "image",
    "image": "registry.cn-hangzhou.aliyuncs.com/goodrain/plugins-tcm:v5.5.0-release",
    "config_group": [
      {
        "service_meta_type": "upstream_port",
        "injection": "auto",
        "config_name": "端口是否开启分析",
        "options": [
          {
            "attr_name": "OPEN",
            "attr_type": "radio",
            "attr_default_value": "YES",
            "is_change": true,
            "attr_alt_value": "YES,NO",
            "attr_info": "是否开启当前端口分析，用户自助选择服务端口",
            "protocol": "http,mysql"
          }
        ]
      }
    ]
  }
}
```

#### 开通插件

入口：

1. 企业插件=>本地插件库（共享插件）
2. 企业插件=>插件市场
3. 团队插件=>共享插件
4. 团队插件=>插件市场

```
TenantPlugin => 'tenant_plugin'{
	ID 				int
  TenantID 		string
  PluginID		string
  Region			string
	PluginName		string
	AliasName		string	"显示名"
	Category		string	"类型，枚举：一般类型|初始化类型|入口网络|出口网络|出口入口共治网络|性能分析|监控"
	Image			string	"镜像名"
	Command			string	"执行命令，可为空；多为初始化容器时使用。"
	CPU				string	"例如：100m"
	Memory			string	"例如：128M" 
	Desc			string	"描述信息"
	CreateUser		string
	ConfigGroups	[]ConfigGroup
}
PluginBuildVersion => 'plugin_build_version'{
	ID				int
	TenantID 		string
	PluginID		string
	Region			string
	UserID			string
	UpdateInfo		string
	BuildVersion	string
	BuildStatus		string	"build_success|unbuild"
	PluginVersionStatus	string	"fixed|unfixed"
	MinCPU			int
	MinMemory		int
	EventID			string
	BuildCMD		string
	ImageTag		string
	CodeVersion		string
	BuildTime		time
}
PluginConfigGroup => 'plugin_config_group'{
	ID					int
	PluginID			string
	BuildVersion		string
	ConfigName			string
	ServiceMetaType		string	"downstream_port|upstream_port|un_define"
	Injection			string
	CreateTime			time
}
PluginConfigItems => 'plugin_config_items' {
	ID				int
	PluginID		string
	BuildVersion	string
	ServiceMetaType		string
	AttrName		string
	AttrType		string	"string|radio|checkbox"
	AttrAltValue	string
	AttrDefaultValue	string
	IsChange		bool
	AttrInfo		string
	Protocol		string	"多选：http|grpc"
	CreateTime		time
}
```
