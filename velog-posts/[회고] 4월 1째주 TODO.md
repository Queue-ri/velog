<blockquote>
<h4 id="기간">기간</h4>
<p><code>25-04-01(화)</code> ~ <code>25-04-06(일)</code></p>
</blockquote>
<br />

<h3 id="😘개발">😘개발</h3>
<blockquote>
<h4 id="리딩한-칼럼들">리딩한 칼럼들</h4>
</blockquote>
<ul>
<li><p><a href="https://news.hada.io/topic?id=19816">HTTP/3가 널리 지원되지만 실제로는 거의 사용되지 않는 이유</a></p>
<ul>
<li>롱테일 트래픽이 전체의 67%씩이나 차지한다는 점이 흥미로움.</li>
<li>꽤나 경계할만한 이슈라고 생각함. 빅테크 기업이 표준을 주도하는 일은 이전부터 있었지만, HTTP/3는 오픈소스 생태계의 분열을 야기하면서도 장기적으로는 특정 기업의 독점 또는 그들만의 리그를 구축할 여지가 있음.</li>
</ul>
</li>
<li><p><a href="https://medium.com/ssgtech/ssg-%EC%9E%90%EB%8F%99%ED%99%94%EC%84%BC%ED%84%B0-%EC%9A%B4%EC%98%81%EC%8B%9C%EC%8A%A4%ED%85%9C%EC%97%90%EC%84%9C-%EB%B6%84%EC%82%B0-%EB%9D%BD%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95-7c3aa89ec5c8">SSG 자동화센터 운영시스템에서 분산 락을 사용하는 방법</a></p>
<ul>
<li>요약하자면 이런 내용임.</li>
<li>1️⃣ <strong>비관적 락 안쓴 이유</strong>: 수행하는 여러 작업 유형 중 동시성 문제 일으키는건 일부인데 비관적 락은 테이블 자체에 락을 걸어서 락 안걸어도 되는 작업까지 대기시켜 성능을 저하시키므로</li>
<li>2️⃣ <strong>낙관적 락 안쓴 이유</strong>: 개발자가 따로 구현해주어야 하는 부분이 있는데 여기에 할애할 시간이 없음</li>
<li>3️⃣ <strong>그래서 사용했던 방식</strong>: 동시성 처리를 위한 진행중 작업 유무 기록용 테이블을 별도로 두고 조회함.<ul>
<li>해당 방식의 문제: 조회가 동시에 일어나면 동시성 문제가 똑같이 발생함</li>
</ul>
</li>
<li>4️⃣ <strong>개선한 방식</strong>: Redis를 통한 분산 락 구현<ul>
<li>Redis는 싱글스레드라 동시성 문제가 발생하지 않음</li>
<li>분산 락 로직이 CCC(Cross-cutting concern)라서 이를 Aspect 클래스로 모듈화하여 적용함. 그런데 <code>@Transactional</code>도 내부적으로 Advice를 등록함. 목적은 트랜잭션 작업 외부에 락 조작이 들어가는 것이므로, 락 Advice가 먼저 수행되도록 <code>@Order</code>를 통해 Advisor 간 순서를 조정함.</li>
</ul>
</li>
<li>이전 프로젝트에서 비관적 락으로 부하 테스트 걸었는데 어느 수준에서 데드락이 걸려서, 해당 방식으로 개선해볼 수 있을 것 같음.</li>
</ul>
</li>
<li><p><a href="https://news.hada.io/topic?id=20067">PostgreSQL 사용 시 도움 되는 패턴들</a></p>
<ul>
<li>JSON 타입까진 써봤는데 N+1 문제 없이 쿼리로 JSON을 반환한다고여? DB가? 완전 포친놈인듯</li>
</ul>
</li>
<li><p><a href="https://helloworld.kurly.com/blog/deliveryproductteam-culture-1/">딜리버리 프로덕트 개발팀의 개발문화 - 로그 &amp; 알람편</a></p>
<ul>
<li>이번에 로그 대시보드를 만들어보니 해당 내용이 완전 잘 이해됨. 로깅도 체계가 필요하고 컨벤션이 필요함. <code>WARN</code>도 의미 있는게 있고 없는게 있는데다 MSA 환경에서 <code>INFO</code>로 request 추적하려면 다른 쓰잘데기 없는 로그를 열심히 필터링해줘야 함.</li>
<li>그래서 생각해본게 custom log와 custom log level임.<ul>
<li>스프링 logback을 설정해서 level별로 파일을 분리하고, 각 파일당 별도의 패턴을 적용해서 파싱할 수 있지 않을까 함. (안되면 말고😗)</li>
</ul>
</li>
</ul>
</li>
<li><p><a href="https://oliveyoung.tech/2025-02-14/oy-global-mall-address/">올리브영 글로벌몰 주소 자동완성 및 검증 솔루션 도입기</a></p>
<ul>
<li>주소 자동완성 기능을 추가했을 뿐인데 리드타임 0.5일 단축으로 이어질 정도로 영향력이 컸던게 신기함. 비즈니스 문제를 잘 잡은듯.</li>
</ul>
</li>
</ul>
<blockquote>
<h4 id="리딩한-정보들">리딩한 정보들</h4>
</blockquote>
<ul>
<li><a href="https://news.hada.io/topic?id=19791">오픈소스 Notion 대체제</a></li>
<li><a href="https://news.hada.io/topic?id=20133">Shezem-rs - Rust 기반의 고속 오디오 지문 인식 시스템</a><ul>
<li>예전부터 audio processing이 서브 관심사였어서 읽어봄.</li>
</ul>
</li>
<li><a href="https://www.marketingideas.com/p/my-weirdest-ab-test-blew-everyones">My weirdest A/B test blew everyone's mind</a><ul>
<li>간단한 UI 수정으로 사람 심리를 산다는 점이 흥미로웠다.</li>
</ul>
</li>
</ul>
<blockquote>
<h4 id="기술면접-질문-리뷰">기술면접 질문 리뷰</h4>
</blockquote>
<ul>
<li><a href="https://www.maeil-mail.kr/question/28">JPA의 ddl-auto 옵션은 각각 어떤 동작을 하고 어떤 상황에서 사용해야 할까요?</a><ul>
<li><code>none</code>: 아무것도 안함 (✅Production-ok)</li>
<li><code>validate</code>: 스키마 매핑 확인 (✅Production-ok)</li>
<li><code>update</code>: 엔티티 업데이트 시 스키마 반영</li>
<li><code>create</code>: 매번 스키마 초기화</li>
<li><code>create-drop</code>: 매번 스키마 초기화 + 종료시 스키마 삭제</li>
</ul>
</li>
<li><a href="https://www.maeil-mail.kr/question/29">엔티티 매니저에 대해 설명해주세요.</a><ul>
<li>엔티티 매니저는 영속성 컨텍스트와 상호작용하여 영속 로직을 수행함.</li>
<li>1️⃣ 엔티티 상태 변경: 비영속, 영속, 준영속, 삭제</li>
<li>2️⃣ 1차 캐시로부터 엔티티 조회</li>
<li>3️⃣ 쓰기 지연 저장소에 있는 쿼리들을 flush하여 DB와 동기화</li>
<li>4️⃣ JPQL, Native Query로 직접 DB로부터 데이터 불러오기</li>
</ul>
</li>
<li><a href="https://www.maeil-mail.kr/question/49">JPA의 N + 1 문제에 대해서 설명해주세요.</a><ul>
<li>연관 관계가 매핑된 엔티티를 조회할시, 조회된 개수(N)만큼 연관관계의 조회 쿼리가 추가로 발생하는 문제</li>
<li><strong>해결방안</strong><ul>
<li><code>fetch join</code>: 연관 관계 엔티티를 한번에 즉시 로딩</li>
<li><code>@EntityGraph</code>: 비슷한 효과. 쿼리 메소드에 해당 어노테이션을 추가</li>
</ul>
</li>
</ul>
</li>
<li><a href="https://www.maeil-mail.kr/question/50">자바에서 Checked Exception과 Unchecked Exception에 대해서 설명해주세요.</a><ul>
<li><strong>Checked Exception</strong>: 컴파일 타임에 확인, 반드시 처리해야 하는 예외<ul>
<li><code>throws</code>로 호출자에게 예외를 위임하거나</li>
<li><code>try-catch</code>로 예외처리해야 함.</li>
<li><code>IOException</code>, <code>SQLException</code>이 여기에 해당</li>
</ul>
</li>
<li><strong>Unchecked Exception</strong>: 런타임 예외, 컴파일러가 처리를 강제하지 않음.<ul>
<li>대부분 프로그래머의 실수로 발생</li>
<li><code>RuntimeException</code>을 상속한 예외들이 여기에 해당</li>
</ul>
</li>
</ul>
</li>
</ul>
<blockquote>
<h4 id="입사지원서-작성">입사지원서 작성</h4>
</blockquote>
<p>CJ올리브네트웍스 자소서가 4월 3일에 마감이었다. 어찌저찌 작성했다.
여담인데, 가입시 이메일 인증코드 칸에서 우클릭하면 뜬금없이 중국어가 나온다(?)</p>
<p><img alt="" src="https://velog.velcdn.com/images/qriosity/post/0dde72b8-dc5b-4133-a6b3-a14ea6d0bb1a/image.png" /></p>
<p>그리고 직무관련 대학교 수강과목 3개 쓰라고 하는데 이건 완벽했던 것 같다.
내 인생 모든게 좀 이랬으면 ㅎ.......</p>
<p><img alt="" src="https://velog.velcdn.com/images/qriosity/post/46491814-73b4-44e4-a2f0-4d176c5735c0/image.png" /></p>
<br />

<h3 id="⚡액티비티">⚡액티비티</h3>
<blockquote>
<h4 id="카페-방문">카페 방문</h4>
<p><del>작성 중</del></p>
</blockquote>
<blockquote>
<h4 id="산책">산책</h4>
<p><code>25-04-01</code>: 21:26 ~ 23:29
<code>25-04-03</code>: 20:14 ~ 22:14
<code>25-04-04</code>: 20:23 ~ 22:23
<code>25-04-05</code>: 21:08 ~ 22:31</p>
</blockquote>
<h4 id="250401">250401</h4>
<ul>
<li>24시 편의점(이었던 것)</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/qriosity/post/f6cd7214-dd8d-4e7f-9e6d-de7a197e6758/image.png" /></p>
<h4 id="250403">250403</h4>
<ul>
<li>남위례역까지 갔다왔다. 엄마가 이 사실을 알면 안된다... 이걸 쓰는 지금도 엄마가 주변에 서성이고 있다. 식은땀이 흐른다.</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/qriosity/post/9136d89e-aa29-40f0-b7db-6593a1674f71/image.png" /></p>
<h4 id="250405">250405</h4>
<ul>
<li>비가 내려서 한적했다.</li>
<li>왼쪽 사진 사실 3일자껀데 사진이 세로로 길쭉해서 둘이 붙였음 ㅇㅅaㅇ</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/qriosity/post/ff144e64-7a5d-43a8-b4bf-431ae07439dc/image.png" /></p>
<br />

<h3 id="🐾회고의-장점">🐾회고의 장점</h3>
<ul>
<li>생각보다 일주일에 한 것이 별로 없다는 사실을 쉽게 인지하게 해줌!</li>
<li>개발 외의 회고 내용 채우려고 뭐라도 움직이게 됨.</li>
</ul>