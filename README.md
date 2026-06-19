# #!/bin/bash
# ============================================================
# 2026年最強 Claude Code フォルダ構成セットアップスクリプト
# ============================================================
# 使い方:
#   chmod +x setup_claude_code_structure.sh
#   ./setup_claude_code_structure.sh [プロジェクト名]
#
# 引数を省略した場合は "your-project" という名前で作成されます。
# ============================================================

set -e

PROJECT_NAME="${1:-your-project}"

echo "📁 プロジェクト '$PROJECT_NAME' を作成します..."

# ルートフォルダ
mkdir -p "$PROJECT_NAME"
cd "$PROJECT_NAME"

# --- ルート直下のファイル ---
cat > CLAUDE.md << 'EOF'
# CLAUDE.md

チーム用の指示ファイルです。リポジトリにコミットされ、全員に共有されます。

## プロジェクト概要
(ここにプロジェクトの概要を記載)

## コーディング規約
(ここにルールを記載)
EOF

cat > CLAUDE.local.md << 'EOF'
# CLAUDE.local.md

個人用の上書き設定ファイルです。
.gitignore に追加し、コミットしないようにしてください。
EOF

# --- .claude/ ディレクトリ ---
mkdir -p .claude/commands
mkdir -p .claude/rules
mkdir -p .claude/skills/security-review
mkdir -p .claude/skills/deploy
mkdir -p .claude/agents

# settings.json (権限・設定: コミット対象)
cat > .claude/settings.json << 'EOF'
{
  "permissions": {
    "allow": [],
    "deny": []
  }
}
EOF

# settings.local.json (個人用設定: .gitignore対象)
cat > .claude/settings.local.json << 'EOF'
{
  "permissions": {
    "allow": [],
    "deny": []
  }
}
EOF

# --- commands/ : カスタムスラッシュコマンド ---
cat > .claude/commands/review.md << 'EOF'
# /project:review

コードレビューを実行するカスタムコマンドです。
EOF

cat > .claude/commands/fix-issue.md << 'EOF'
# /project:fix-issue

Issueを修正するためのカスタムコマンドです。
EOF

cat > .claude/commands/deploy.md << 'EOF'
# /project:deploy

デプロイを実行するためのカスタムコマンドです。
EOF

# --- rules/ : モジュール化された指示ファイル ---
cat > .claude/rules/code-style.md << 'EOF'
# コードスタイルガイド

(ここにコードスタイルのルールを記載)
EOF

cat > .claude/rules/testing.md << 'EOF'
# テスト方針

(ここにテストに関するルールを記載)
EOF

cat > .claude/rules/api-conventions.md << 'EOF'
# API命名・設計規約

(ここにAPI設計のルールを記載)
EOF

# --- skills/ : 自動実行されるワークフロー ---
cat > .claude/skills/security-review/SKILL.md << 'EOF'
# Security Review Skill

セキュリティレビューを自動実行するスキルです。
EOF

cat > .claude/skills/deploy/SKILL.md << 'EOF'
# Deploy Skill

デプロイ作業を自動実行するスキルです。
EOF

# --- agents/ : サブエージェントの役割(ペルソナ) ---
cat > .claude/agents/code-reviewer.md << 'EOF'
# Code Reviewer Agent

コードレビューを専門に行うサブエージェントのペルソナ定義です。
EOF

cat > .claude/agents/security-auditor.md << 'EOF'
# Security Auditor Agent

セキュリティ監査を専門に行うサブエージェントのペルソナ定義です。
EOF

# --- .gitignore: 個人用ファイルを除外 ---
cat > .gitignore << 'EOF'
CLAUDE.local.md
.claude/settings.local.json
EOF

cd ..

echo ""
echo "✅ 完了しました！ 以下の構成が作成されました:"
echo ""
find "$PROJECT_NAME" -print | sed -e "s|[^/]*/|  |g"