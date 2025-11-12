# 安全审计报告（Awesome Claude Skills）

## 审计范围与方法
- 范围：仓库根目录全部技能与脚本（Python、Shell、示例代码）。
- 方法：Secrets 扫描、危险模式静态检查、关键脚本人工审阅、改进建议与验证清单。

## 发现摘要
- 明文密钥：未发现真实凭据，示例均为占位符（如 `ANTHROPIC_API_KEY=your_api_key`）。
- 危险调用：
  - 命令执行 shell=True（中风险）：`webapp-testing/scripts/with_server.py:71` 用于支持 `cd &&` 链式命令，若传入不受信输入存在注入风险。
  - 清理命令（低-中风险）：`artifacts-builder/scripts/bundle-artifact.sh:36` 使用 `rm -rf` 清理构建产物，需确保在受控目录运行。
- XML 解析（改进项）：
  - 使用标准 ET 的位置（建议替换为 defusedxml）：
    - `mcp-builder/scripts/evaluation.py:13`
    - `document-skills/docx/ooxml/scripts/validation/redlining.py:32`、`document-skills/docx/ooxml/scripts/validation/redlining.py:84`
  - 已安全：`document-skills/docx/ooxml/scripts/pack.py` 使用 `defusedxml.minidom`。
- 未发现：不安全反序列化（pickle/yaml.load）、HTTP 证书校验禁用、危险解压（extractall）、外部下载（curl/wget）。

## 建议与修复
- 限定 shell=True 使用场景（中）：
  - 在 `webapp-testing/scripts/with_server.py` 明确“仅用于受信输入”；可增加对列表参数的支持并优先 `shell=False`。
- 加固 XML 解析（中）：
  - 将上述 ET 导入替换为 `defusedxml.ElementTree as ET`，防范实体扩展/外部实体等攻击。
- 提升 Shell 脚本健壮性（中）：
  - 在 `artifacts-builder/scripts/*.sh` 统一启用 `set -euo pipefail`；固定关键依赖版本，减少供应链漂移；输出提示“在项目根目录、受控环境运行”。
- 依赖与扫描（建议）：
  - 锁定 Python/Node 依赖版本；在 CI 增加 secrets 扫描（gitleaks/trufflehog）与 Python 安全静态检查（bandit）。

## 验证清单（执行者用）
- 修改 ET 导入并回归跑脚本：
  - `mcp-builder/scripts/evaluation.py:13`
  - `document-skills/docx/ooxml/scripts/validation/redlining.py:32`、`84`
- 为 `with_server.py` 增加安全提示与（可选）列表参数支持；本地验证多服务起停场景。
- 为 `artifacts-builder` 脚本补充 `set -euo pipefail`、版本钉死与运行提示；重复构建验证无副作用。
- 配置 CI 扫描：secrets、bandit（Python）、基础 SCA（若允许）。

## 结论
- 本仓库整体无明文凭据与明显高危实现；存在少量“可被误用”的开发脚本与 XML 解析改进点。采纳上述修复与流程加固后，可显著降低命令注入、XML 实体扩展与供应链漂移风险。

