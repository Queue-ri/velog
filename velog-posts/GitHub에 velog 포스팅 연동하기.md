<h2 id="📌-github-리포지토리-생성">📌 GitHub 리포지토리 생성</h2>
<ul>
<li>public으로 생성합니다.</li>
</ul>
<p><img alt="repo" src="https://velog.velcdn.com/images/qriosity/post/f95c7300-15a1-4571-b950-7515acb01f65/image.png" /></p>
<br />

<h2 id="📌-python-스크립트-작성">📌 Python 스크립트 작성</h2>
<ul>
<li>파일명: <code>./scripts/update_blog.py</code></li>
<li><code>[벨로그_아이디]</code> 부분에 자신의 벨로그 아이디를 기입합니다.<pre><code class="language-python">import feedparser
import git
import os
</code></pre>
</li>
</ul>
<h1 id="벨로그-rss-피드-url">벨로그 RSS 피드 URL</h1>
<p>rss_url = '<a href="https://api.velog.io/rss/@%5B%EB%B2%A8%EB%A1%9C%EA%B7%B8_%EC%95%84%EC%9D%B4%EB%94%94%5D'">https://api.velog.io/rss/@[벨로그_아이디]'</a></p>
<h1 id="깃허브-레포지토리-경로">깃허브 레포지토리 경로</h1>
<p>repo_path = '.'</p>
<h1 id="velog-posts-폴더-경로">'velog-posts' 폴더 경로</h1>
<p>posts_dir = os.path.join(repo_path, 'velog-posts')</p>
<h1 id="velog-posts-폴더가-없다면-생성">'velog-posts' 폴더가 없다면 생성</h1>
<p>if not os.path.exists(posts_dir):
    os.makedirs(posts_dir)</p>
<h1 id="레포지토리-로드">레포지토리 로드</h1>
<p>repo = git.Repo(repo_path)</p>
<h1 id="rss-피드-파싱">RSS 피드 파싱</h1>
<p>feed = feedparser.parse(rss_url)</p>
<h1 id="각-글을-파일로-저장하고-커밋">각 글을 파일로 저장하고 커밋</h1>
<p>for entry in feed.entries:
    # 파일 이름에서 유효하지 않은 문자 제거 또는 대체
    file_name = entry.title
    file_name = file_name.replace('/', '-')  # 슬래시를 대시로 대체
    file_name = file_name.replace('\', '-')  # 백슬래시를 대시로 대체
    # 필요에 따라 추가 문자 대체
    file_name += '.md'
    file_path = os.path.join(posts_dir, file_name)</p>
<pre><code># 파일이 이미 존재하지 않으면 생성
if not os.path.exists(file_path):
    with open(file_path, 'w', encoding='utf-8') as file:
        file.write(entry.description)  # 글 내용을 파일에 작성

    # 깃허브 커밋
    repo.git.add(file_path)
    repo.git.commit('-m', f'docs: Add {entry.title}')</code></pre><h1 id="변경-사항을-깃허브에-푸시">변경 사항을 깃허브에 푸시</h1>
<p>repo.git.push()</p>
<pre><code>
&lt;br/&gt;

## 📌 PAT 생성
1) 우상단의 프로필 클릭 -&gt; Settings
![setting](https://velog.velcdn.com/images/qriosity/post/63a8ed62-82f6-45c3-b0d5-f59303146833/image.png)

---

2) 좌측 사이드바 하단의 Developer settings
![developer_setting](https://velog.velcdn.com/images/qriosity/post/8b3834c8-43a0-47b5-889e-dc9f7db71ba3/image.png)

---

3) Personal access tokens - Tokens (classic) 에서 Generate new token (classic) 클릭
![pat_menu](https://velog.velcdn.com/images/qriosity/post/10cc7eef-e98f-46bd-9a74-f7568202434d/image.png)

---

4) PAT 설정
- **Note**는 자유롭게 기입합니다.
- **Expiration**은 보안상 `No expiration`이 권장되지 않지만, 저처럼 하고 싶은 다른 일들이 많고 위험을 즐기시는 분들이라면 괜찮습니다.
- **Scope**은 `workflow`에 체크해줍니다.

![pat_setting](https://velog.velcdn.com/images/qriosity/post/c0946ff4-fe09-4ff9-8989-8899a4f3dbb1/image.png)

---

5) 생성된 PAT 복사하기
- 해당 페이지를 나가면 더이상 확인할 수 없으므로, 바로 복사해줍니다.

![pat_generated](https://velog.velcdn.com/images/qriosity/post/e05347d0-e7c9-4fb7-bbb6-7dabb49114da/image.png)

---

6) 리포지토리 Settings - Secrets and variables - Actions에서 New repository secret 버튼 클릭
- 우상단 프로필 눌러서 진입하는 계정 Settings랑 헷갈리지 마세요!

![repo_secret_menu](https://velog.velcdn.com/images/qriosity/post/74473133-23f5-4536-909e-280bcab942ce/image.png)


---

7) 복사한 PAT 붙여넣기
- **Name**은 하단에서 작성할 CI 스크립트에 사용됩니다.

![repo_secret_paste](https://velog.velcdn.com/images/qriosity/post/712ba9b2-171f-4668-abc2-a45cdccbae8c/image.png)

---

8) GitHub Actions 권한 설정
- 리포지토리 Settings - Actions - General에서 차례대로 3가지를 설정합니다.

1️⃣ `Allow all actions and reusable workflows` 선택 후 하단의 **Save** 버튼 클릭

![actions_general_setting_0](https://velog.velcdn.com/images/qriosity/post/4c039802-c364-4c7b-86ae-9e03a098a40f/image.png)

2️⃣ 조금 스크롤 내려서 `Require approval for all external contributors` 선택 후 **Save** 버튼 클릭

![actions_general_setting_1](https://velog.velcdn.com/images/qriosity/post/38e2b858-d36a-40dc-89eb-c1414dd3b288/image.png)

3️⃣ `Read and write permissions` 선택 및 `Allow GitHub Actions to create and approve pull requests` 체크 후 **Save** 버튼 클릭

![actions_general_setting_2](https://velog.velcdn.com/images/qriosity/post/36b72268-ea46-4c1d-acd6-ee83610b9296/image.png)

&lt;br/&gt;

## 📌 CI 스크립트 작성
- 파일명: `./.github/workflows/update_blog.yml`
- `[깃허브_아이디]` 부분에 자신의 GitHub username을 기입합니다.
- cron의 경우 GitHub Actions는 UTC 기준으로 실행되므로, 한국 시간 기준을 생각한다면 UTC로 변환하여 작성해야 합니다.
    - [KST → UTC 변환기](https://www.freeconvert.com/time/kst-to-utc)

```yaml
name: Update Velog Posts

on:
  push:
      branches:
        - main
  schedule:
    - cron: '0 15 * * *' # UTC 15:00 (KST 00:00)

jobs:
  update_blog:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout
      uses: actions/checkout@v2

    - name: Push changes
      run: |
        git config --global user.name 'github-actions[bot]'
        git config --global user.email 'github-actions[bot]@users.noreply.github.com'
        git push https://${{ secrets.VELOG_CI_PAT }}@github.com/[깃허브_아이디]/velog.git

    - name: Set up Python
      uses: actions/setup-python@v2
      with:
        python-version: '3.x'

    - name: Install dependencies
      run: |
        pip install feedparser gitpython

    - name: Run script
      run: python scripts/update_blog.py</code></pre><br />

<h3 id="✅-ci-결과-확인">✅ CI 결과 확인</h3>
<p>무사히 pass 했다면, 추후 cron 설정에 따라 벨로그 포스트들이 <code>velog-posts</code> 폴더에 커밋됩니다.</p>
<p><img alt="ci_pass" src="https://velog.velcdn.com/images/qriosity/post/22bbd29e-0e2b-43a5-a9d0-bbed73b8e924/image.png" /></p>