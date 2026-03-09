```markdown
# Retro Capture

## 対象
- PR: {url or number}
- CI Run: {url or id}
- takt Run ID: {run-id}
- 比較対象: {spec/tasks/design/reports}

## 期待結果
- {本来満たすべき完了条件}

## 実際の結果
- {実際に起きた事象}

## 収集証跡
| 種別 | 参照 | 要点 |
|------|------|------|
| CIログ | {run/job/step} | {失敗要点} |
| PR差分 | {path or summary} | {要点} |
| taktレポート | {report name} | {要点} |
| takt遷移ログ | `.takt/runs/{run-id}/logs/*.jsonl` | {要点} |

## 遷移監査（takt run）
| 時刻 | movement | event | condition/judge | 根拠 |
|------|----------|-------|-----------------|------|
| {ts} | {implement} | {step_start} | {-} | {jsonl抜粋} |
| {ts} | {supervise} | {COMPLETE} | {condition} | {jsonl抜粋} |

## 遷移監査所見
- 誤判定候補: {なし / 内容}
- 監査不能箇所: {なし / 内容}

## ギャップ一覧
| 観点 | 期待 | 実際 | 差分 |
|------|------|------|------|
| {lint/test/build} | {...} | {...} | {...} |

## 信頼度
- 直接証跡: {件数}
- 仮説: {件数と内容}

## 判定
{証跡収集完了 / 証跡不足で分析不能}
```
