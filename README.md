# drama-pipeline

漫剧 / AI 短剧从剧本到视频的交互式总路由 skill。它负责在每个阶段列出可选方法，等待用户选择，再加载对应的专用 skill 执行。

## 内容

- `skill/SKILL.md`：可安装的总路由 skill。
- `skill/agents/openai.yaml`：Codex skill 元数据。
- `docs/production-workflow.md`：完整制作流程、阶段产物和批量生成前的硬性检查。
- `docs/lessons.md`：已验证的失败 → 根因 → 修复 → 验证记录。
- `docs/open-issues.md`：尚未完成验证的问题，不冒充已验证经验。
- `docs/production-history.md`：本项目已完成事项与当前状态。
- `CHANGELOG.md`：skill 和流程的更新记录。

## 设计原则

1. 每个阶段都先问用户采用哪种方法，不自动跨阶段。
2. 生成视频前先展示最终提示词，等待确认；批量提交前先做单段预检。
3. 角色身份以确认过的参考图为唯一基准，声线、空间位置、视线方向和动作因果都要显式锁定。
4. 只把已经定位根因并完成验证的经验写入 `docs/lessons.md`；其余放入 `docs/open-issues.md`。
5. 每次批量生成后比较“计划数量”和“实际文件”，并核验分辨率、帧率、时长和音频流。

## 安装

将 `skill/` 目录复制到 Codex skills 目录，或按本机的 skill 安装流程安装。该仓库只归档总路由；它依赖的专业 skills 仍按各自仓库或本机安装状态加载。

## 持续归档

每次漫剧制作出现新的、可复用且已验证的问题修复时，更新：

- `skill/SKILL.md`：流程规则发生变化时更新；
- `docs/lessons.md`：已验证经验；
- `docs/open-issues.md`：未验证假设；
- `CHANGELOG.md`：说明本次变化和验证范围；
- `docs/production-history.md`：补充实际生产节点和交付证据。

