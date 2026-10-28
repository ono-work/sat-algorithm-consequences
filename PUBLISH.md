# Publication guide / 公開手順

## Selected target / 選択済みの公開先

| Setting / 設定 | Value / 値 |
| --- | --- |
| Repository / リポジトリ | [ono-work/sat-algorithm-consequences](https://github.com/ono-work/sat-algorithm-consequences) |
| Visibility / 公開範囲 | Public / 公開 |
| GitHub account / GitHubアカウント | `agent-maria` (public user ID: `308191943`) |
| Branch / ブランチ | `main` |
| License / ライセンス | Unselected / 未選定 |

These settings prepare **Parameterized and Exact Algorithms from Faster SAT** for publication; they do not record a completed upload. 以下は公開のために準備した設定と手順であり、アップロード完了の記録ではありません。

## 日本語

### 1. 収録内容を準備する

README、全体一覧、FPT一覧、厳密アルゴリズム一覧、仮定を英日両方で確認します。問題の版、k/n/m/n+mの尺度、疎密や重みの制限を保ち、`source-corollary` とカタログ側の導出、古典的基準との比較と最新最良上界の調査を区別します。draftはdraftのまま共有でき、人間の確認済みという表示を公開条件として要求しません。

```bash
python3 tools/render_catalog.py
python3 tools/render_catalog.py --check
```

このチェックはスキーマと生成物の一致を確認し、証明や元のSAT定理の正しさを認定するものではありません。ソースデータと生成された英日ページを一緒に含めます。

### 2. GitHubで認証し、使用アカウントを確認する

GitHub CLIのWeb・デバイス認証を使い、GitHubの画面で `agent-maria` としてログインします。パスワードやトークンをチャットやリポジトリへ記載する必要はありません。

```bash
gh auth login --hostname github.com --git-protocol https --web
gh api --hostname github.com user --jq '{login: .login, id: .id}'
```

APIの結果が `agent-maria`、ID `308191943` であることを確認してから作成・pushへ進みます。別のアカウントだった場合は、GitHub CLIで認証アカウントを切り替えて再確認します。`ono-work` 内での作成・更新には、そのアカウントの対応する権限を使用します。

**認証アカウントとコミットの作成者情報は別です。** GitHub認証は作成・pushに使う権限を決め、Gitの `user.name` と `user.email` はコミットに記録される名前とメールアドレスを決めます。既存の作成者設定を尊重し、GitHubのログイン名を設定するだけで認証できるとは扱いません。

### 3. 既存リポジトリを確認し、公開する

認証済みの状態で対象を確認します。取得エラーだけを不存在の証拠にしません。

```bash
gh repo view ono-work/sat-algorithm-consequences --json nameWithOwner,visibility,defaultBranchRef,url
```

既存リポジトリがある場合は、内容・履歴・remoteを確認し、既存の作業手順で変更を取り込みます。作り直しや履歴の上書きは行いません。ローカル側も既にGit管理下なら、既存のブランチ・remoteを確認して使います。

以下は、ローカルがまだGit管理下でない場合だけの初期化例です。コミットの作成者情報が設定された環境で実行します。

```bash
git init -b main
git add .
git commit -m "Add bilingual SAT algorithm consequences catalog"
```

**認証結果が上記の `agent-maria` と一致し、対象リポジトリを新規作成する場合だけ**、コミットを含むローカルリポジトリの `main` ブランチから次を実行します。既存の `origin` があれば先に内容を確認し、置き換えません。

```bash
gh repo create ono-work/sat-algorithm-consequences --public --source=. --remote=origin --push
```

実行後にGitHub側のURL、公開範囲、`main` と収録ファイルを確認してから、公開完了を記録します。アーカイブには `data/`、`tools/`、両言語の生成物、手順・出典の記録を含め、`__pycache__/` などの作業用ファイルを除きます。公開・push・PR作成などは、その時点の利用者の指示に従います。

## English

### 1. Prepare the contents

Inspect the README, overall catalog, FPT index, exact-algorithm index, and assumptions in both languages. Preserve variants, size measures, density and weight restrictions, source provenance, and the distinction between classical benchmarks and latest-best surveys. Drafts may be shared with accurate labels; human review is not a publication prerequisite. Run the render and check commands above, and include data and generated pages together. The check validates schema and generated-content consistency, not proofs or the underlying SAT result.

### 2. Authenticate and verify the account

Use GitHub CLI's web/device flow above and sign in to GitHub as `agent-maria`. Credentials are entered through GitHub, without placing passwords or tokens in chat or the repository. Before creation or push, confirm that the authenticated API response has login `agent-maria` and ID `308191943`; switch the authenticated CLI account and check again if necessary. Repository operations use that account's permissions in `ono-work`.

**Authentication and Git commit identity are separate.** Authentication determines which account's permissions apply to creation and push. Git's `user.name` and `user.email` determine the author metadata recorded in commits. Respect an existing author configuration; setting a Git username does not authenticate to GitHub.

### 3. Check for an existing repository, then publish

Run the authenticated repository inspection above. A failed lookup alone does not establish that the repository is absent. If it exists, inspect its contents, history, visibility, and remotes, then integrate through its existing workflow without recreating it or overwriting history. Preserve an existing local Git repository as well.

Use the initialization example only for a directory that is not already under Git and has an appropriate commit identity configured. Use the `gh repo create` command only after the authenticated identity matches `agent-maria`, when a new target repository is needed, and from the prepared local `main` branch with commits. Inspect an existing `origin` instead of replacing it. After the command succeeds, verify the remote URL, public visibility, `main`, and files before recording publication as complete.

Archives should include data, tools, both languages, and methods/source records, while excluding temporary files such as `__pycache__/`. Follow the current user task's authorization for publication, pushes, and pull requests.

## Official references / 公式資料

- [GitHub CLI: web authentication](https://cli.github.com/manual/gh_auth_login)
- [GitHub CLI: create a repository from local source](https://cli.github.com/manual/gh_repo_create)
- [GitHub Docs: Git usernames and commit identity](https://docs.github.com/en/get-started/git-basics/setting-your-username-in-git)
