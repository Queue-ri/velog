<h1 id="💥-문제-상황">💥 문제 상황</h1>
<img src="https://velog.velcdn.com/images/qriosity/post/858d41c9-6443-4c42-8f98-6411981a78d6/image.png" />
<center><i><sub><span style="color: #9c9c9c;">님아 그 강을 건너지 마오...</span></sub></i></center>

<p>커밋이 <code>A &lt;- B &lt;- C</code> 형태로 쌓인 프로젝트에서, B 커밋의 커밋 메시지가 잘못됨을 깨달았다.</p>
<blockquote>
<p>fix(ci): Modify `grep` statement to handle filepath with spaces (#175)</p>
</blockquote>
<p>커밋 메시지 끝부분에 이슈 번호를 연결하는데
179번이 아니라 175번으로 잘못 적었기 때문이다 🙃............</p>
<p>그래서 <code>git rebase</code>로 커밋 메시지를 수정했는데, 이 과정에서 실수로 메시지 내의 backtick(`)을 날려버렸다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/qriosity/post/166beabd-a18f-4457-9474-1e3e1d3a0b28/image.png" /></p>
<p>흑흑 뒤에 이슈 번호 달린 커밋은 자주 수정하면 안좋은데...</p>
<p>지난번에도 같은 실수를 한 적 있어서, 이번엔 확실하게 기억하고자 포스팅한다.</p>
<br />

<h1 id="✨-해결-방법">✨ 해결 방법</h1>
<h2 id="수정-전">수정 전</h2>
<pre><code class="language-bash">git commit -m &quot;feat: Modify `myVariable` to 42&quot;</code></pre>
<hr />
<h2 id="수정-후">수정 후</h2>
<h3 id="방법-1">방법 1</h3>
<pre><code class="language-bash">git commit -m 'feat: Modify `myVariable` to 42'</code></pre>
<h3 id="방법-2">방법 2</h3>
<pre><code class="language-bash">git commit -m &quot;feat: Modify \`myVariable\` to 42&quot;</code></pre>
<p>의외로 매우 간단한데,</p>
<p>메시지 작성 시에 double quote 대신 single quote를 사용하거나
꼭 double quote를 사용해야 한다면 backtick 앞에 escape 문자를 붙이면 된다.</p>
<br />

<h1 id="📌-결과">📌 결과</h1>
<p><img alt="" src="https://velog.velcdn.com/images/qriosity/post/9de81f2c-a27b-4476-a939-0b89591f5934/image.png" /></p>
<p><code>f61948d</code> 커밋에서 <code>08f9cdb</code> 커밋으로 잘 수정된 것을 확인할 수 있다!</p>
<p><del>물론 이미 push해버린 커밋이라 force push 해야됨</del></p>
<br />

<h1 id="📌-근데-왜-안됐음">📌 근데 왜 안됐음?</h1>
<h2 id="command-substitution">command substitution</h2>
<p><a href="https://stackoverflow.com/questions/71155954/backticks-in-git-commit-message">스택오버플로 고인물</a>의 설명을 인용하자면</p>
<blockquote>
<p>backticks is a way to tell the shell to execute the content, it's called a <strong>command substitution</strong>... (중략)</p>
</blockquote>
<p>backtick은 <strong>command substitution(명령어 치환)</strong>에 사용되는 special character였던 것이다.</p>
<pre><code class="language-bash">$ ls
myfolder  직박구리

$ echo &quot;hello `ls` world&quot;
hello myfolder
직박구리 world</code></pre>
<p>셸 프로그래밍에선 상단과 같이 특정 명령어의 수행결과를 문자열로 입력받을 수 있는데 이를 command substitution이라고 한다.</p>
<p>따라서 필자의 backtick 안에는 <code>grep</code> 명령어가 있었으니, 결과가 아무것도 없어서 공백으로 들어간 것이다.</p>
<h2 id="interpolation">interpolation</h2>
<p>bash manual에 따르면 single quote와 double quote는 interpolation 측면에서 차이가 있다.</p>
<ul>
<li><a href="https://www.gnu.org/software/bash/manual/html_node/Single-Quotes.html">3.1.2.2 Single Quotes</a></li>
<li><a href="https://www.gnu.org/software/bash/manual/html_node/Double-Quotes.html">3.1.2.3 Double Quotes</a></li>
</ul>
<p>interpolation이란 쉽게 말해 일부 special character에 대하여, 문자 값이 아닌 그 의미로 해석하는 것이다.</p>
<p>double quote는 <code>$</code>, <code>`</code>, <code>\</code> 등의 special character를 각각의 의미로 해석하지만</p>
<p>single quote는 또 다른 single quote만 제외하고, 모든 리터럴 값을 생긴 그대로 해석한다고 한다.</p>
<p>이 때문에 single quote 내부에선 <code>\'</code>로 escape 처리하여 single quote를 넣을 수도 없다. <code>\</code> 문자를 escape의 의미로 해석하지 않고, 생긴 그대로 해석하기 때문이다.</p>
<br />

<hr />
<h4 id="🤔-hmm">🤔 Hmm...</h4>
<p>결론을 내리자면, 습관적으로 single quote를 사용하는 것이 좋아보인다.</p>
<p>커밋 메시지 쓸 때 interpolation이 거의 필요 없기도 하고,
실수할 확률을 대폭 낮춰주기 때문이다.</p>