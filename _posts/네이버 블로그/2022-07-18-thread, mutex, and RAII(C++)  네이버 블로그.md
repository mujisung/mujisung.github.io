---
layout: posts
title: #title
categories: [programming, ]
tag: [c++, thread, mutex, 네이버 블로그]  # tag가 추가됨.
toc: true   # 글 오른쪽에 toc가 나온다.
author_profile: false # 글을 누르면 내 소개가 없어짐. true로 하면 얼굴이 나옴.
sidebar:      # 글을 누르면 목차가 나온다.
  nav: "counts" 
search: true     # false라고 하면 글이 검색이 안된다.
---

<div class="notice--info" markdown="1" style='font-size: 20px'>
**motivation:** 네이버 블로그 
</div>




#thread, mutex, and RAII(C++) : 네이버 블로그
<div class="wrap_rabbit pcol2 _param(1) _postViewArea222816438909" id="post-view222816438909">
<!-- Rabbit HTML --><div class="se-viewer se-theme-default" lang="ko-KR">
<!-- SE_DOC_HEADER_START -->
<div class="se-component se-documentTitle se-l-default" id="SE-de1c8b36-c7eb-4631-bed5-951444d6bb45">
<div class="se-component-content">
<div class="se-section se-section-documentTitle se-l-default se-section-align-left">
<!-- -->
<div class="blog2_series">
<a class="pcol2" href="/PostList.naver?blogId=wys000112&amp;categoryNo=15&amp;from=postList" onclick="nclk_v2(this,'pst.category','','');">c++ 정리</a>
</div>
<div class="pcol1">
<!-- -->
<div class="se-module se-module-text se-title-text">
<p class="se-text-paragraph se-text-paragraph-align-" id="SE-a89b3381-8145-4ddf-b763-357b3efc78f8" style=""><span class="se-fs- se-ff-" id="SE-8abb8ef5-ef60-429f-9185-e3dfbfd38107" style=""><!-- -->thread, mutex, and RAII(C++)<!-- --></span></p> </div>
<!-- -->
</div>
<div class="blog2_container">
<span class="writer">
<span class="area_profile"><a class="link" href="https://blog.naver.com/wys000112" onclick="nclk_v2(this,'pst.profile','','');" target="_top"><img alt="프로파일" class="img" src="https://blogpfthumb-phinf.pstatic.net/MjAyMjA1MjVfMTA0/MDAxNjUzNDcxMTU4NTkw.MKx5XZzKhkVnSwLw5O1NM-J45hdDNIrADB_V9VVQBOAg.OkL09v5VWJCO9xIBu4VTEzVASngUXGDvkf4D_exCZsEg.PNG.wys000112/%EB%AC%B4%EC%A7%80%EC%84%B1.png/%25EB%25AC%25B4%25EC%25A7%2580%25EC%2584%25B1.png?type=s1"/></a></span>
<span class="nick"><a class="link pcol2" href="https://blog.naver.com/wys000112" onclick="nclk_v2(this,'pst.username','','');" target="_top">무지성</a></span>
</span>
<i class="dot"> ・ </i>
<span class="se_publishDate pcol2">2022. 7. 18. 2:32</span>
</div>
<div class="blog2_post_function">
<a class="url pcol2 _setClipboard _returnFalse _se3copybtn _transPosition" href="#" id="copyBtn_222816438909" style="cursor:pointer;" title="https://blog.naver.com/wys000112/222816438909">URL 복사</a>
<a class="btn_buddy btn_addbuddy pcol2 _buddy_popup_btn _returnFalse" href="#" onclick="nclk_v2(this,'pst.addnei','','');"><i class="ico"></i> 이웃추가<i class="aline"></i></a>
<div class="overflow_menu">
<a area-expanded="false" area-haspopup="true" class="btn_overflow_menu _open_overflowmenu pcol2 _param(222816438909) _returnFalse" href="#" role="button"><span class="blind">본문 기타 기능</span></a>
<div area-hidden="true" class="lyr_overflow_menu" id="overflowmenu-222816438909">
<a class="naver-splugin btn_splugin share _title_share" data-canonical-url="https://blog.naver.com/wys000112/222816438909" data-likecontentsid="wys000112_222816438909" data-likeserviceid="BLOG" data-logdomain="https://proxy.blog.naver.com/spi/v1/api/shareLog" data-me-display="off" data-oninitialize="splugin_oninitialize(1);" data-option="{baseElement:'_title_spiButton', layerPosition:'outside-bottom', align:'right', marginLeft:0, marginTop:4}" data-style="unity" data-url="https://blog.naver.com/wys000112/222816438909" href="#" id="_title_spiButton" onclick="return false;">
                   공유하기
                <span class="ico_share _title_share_icon"></span>
</a>
<a class="_report _param(https://srp2.naver.com/report?svc=BLG&amp;exit=close&amp;ctype=AA01&amp;cwriterenc=kYvEitdi2PrBGAKclwuh2qP1gNQPaCCiVtK%2BaWlfNLw%3D&amp;ctitle=thread%2C%20mutex%2C%20and%20RAII(C%2B%2B)&amp;cwriter=wys0*****&amp;dark=disable&amp;memtype=Y&amp;env=pc&amp;cnickname=wys0*****&amp;vsvc=BLG&amp;cid=wys000112%40%4051896191%40%40mylog%40%40222816438909) _returnFalse" href="#">신고하기<span class="ico_report"></span></a>
</div>
</div>
<input alt="url" class="copyTargetUrl" style="display:none;" title="URL 복사" type="text" value="https://blog.naver.com/wys000112/222816438909"/>
</div>
<!-- -->
</div>
</div>
</div>
<!-- B2C 상품 -->
<!-- _BLOG_CONTENTS_HEADER_TAIL -->
<!-- SE_DOC_HEADER_END -->
<div class="se-main-container">
<div class="se-component se-text se-l-default" id="SE-8ba5cb6a-1058-4d3a-9dc5-4e4e0edfe363">
<div class="se-component-content">
<div class="se-section se-section-text se-l-default">
<div class="se-module se-module-text">
<!-- SE-TEXT { --><p class="se-text-paragraph se-text-paragraph-align-" id="SE-4d3dd0e0-991f-4ce6-a148-bc322b54c6d2" style=""><span class="se-fs- se-ff-" id="SE-ce8782c2-6a7f-4554-b4c1-5e2d1a02af9c" style="">mutex, thread 훑어보기 + RAII 개념 알아보기</span></p><!-- } SE-TEXT --><!-- SE-TEXT { --><p class="se-text-paragraph se-text-paragraph-align-" id="SE-4cebb459-9732-45b5-a1c7-00821d0fa145" style=""><span class="se-fs- se-ff-" id="SE-df6a28ae-6316-4542-8c5f-6bf940eccc9e" style="">나중에 mutex, thread, condition variable에 대해 공부하고 정리해서 올릴 것이다.</span></p><!-- } SE-TEXT --><!-- SE-TEXT { --><p class="se-text-paragraph se-text-paragraph-align-" id="SE-e65ae676-2cc4-49f5-9330-03aa7c0131e7" style=""><span class="se-fs- se-ff-" id="SE-6d808c5a-d441-4618-82fe-b6ca27144cf1" style="">​</span></p><!-- } SE-TEXT --><!-- SE-TEXT { --><p class="se-text-paragraph se-text-paragraph-align-" id="SE-b4e0ceb5-f4a1-4c44-9739-d2f161bdc3eb" style=""><span class="se-fs- se-ff-" id="SE-e91de6dd-e410-4824-9f14-5005da29cee5" style="">밑의 코드를 보자.</span></p><!-- } SE-TEXT -->
</div>
</div>
</div>
</div> <div class="se-component se-code se-l-code_black" id="SE-47195ca8-3d5d-47b5-80fb-19360c2f91eb">
<div class="se-component-content">
<div class="se-section se-section-code se-l-code_black">
<div class="se-module se-module-code se-fs-fs13">
<div class="se-code-source">
<div class="__se_code_view language-javascript">#include &lt;iostream&gt;
#include &lt;thread&gt;
#include &lt;mutex&gt;
#include &lt;ctime&gt;
typedef unsigned long long ull_t;

int x = 0;
std::mutex m1;
ull_t oddSum = 0, evenSum = 0;
void addEvenNum(ull_t n);
void addOddNum(ull_t n);

int main() {
	// sum with multithreading
	auto time1 = clock();
	std::thread t1(addEvenNum, 19000000);
	std::thread t2(addOddNum, 19000000);
	t1.join();
	t2.join();
	auto time2 = clock();
	std::cout &lt;&lt; "time spent with multithreading: " &lt;&lt; (time2 - time1) &lt;&lt; '\n';
	std::cout &lt;&lt; "evenSum: " &lt;&lt; evenSum &lt;&lt; '\n';
	std::cout &lt;&lt; "oddSum: " &lt;&lt; oddSum &lt;&lt; '\n';


	// sum without multithreading
	oddSum = 0, evenSum = 0;
	auto time3 = clock();
	addEvenNum(19000000);
	addOddNum(19000000);
	auto time4 = clock();	
	std::cout &lt;&lt; "time spent without multithreading: " &lt;&lt; (time4 - time3)&lt;&lt; '\n';
	std::cout &lt;&lt; "evenSum: " &lt;&lt; evenSum &lt;&lt; '\n';
	std::cout &lt;&lt; "oddSum: " &lt;&lt; oddSum &lt;&lt; '\n';
}

void addEvenNum(ull_t n) {
	ull_t condition = 0;
	while (++condition &lt; n) {
		if (condition % 2 == 0) {
			evenSum += condition;
		}
	}
	m1.lock();
	x++;
	m1.unlock();
}

void addOddNum(ull_t n) {
	ull_t condition = 0;
	while (++condition &lt; n) {
		if (condition % 2 == 1) {
			oddSum += condition;
		}
	}
	m1.lock();
	x++;
	m1.unlock();
}</div>
</div>
</div>
</div>
</div>
<script class="__se_module_data" data-module='{"type":"v2_code", "id" : "SE-47195ca8-3d5d-47b5-80fb-19360c2f91eb"}' type="text/data"></script>
</div> <div class="se-component se-text se-l-default" id="SE-a6163cca-76ea-4d01-9a4a-5fabc0f0eeb2">
<div class="se-component-content">
<div class="se-section se-section-text se-l-default">
<div class="se-module se-module-text">
<!-- SE-TEXT { --><p class="se-text-paragraph se-text-paragraph-align-" id="SE-0070a93a-6b7c-43bd-9358-993164f167f2" style=""><span class="se-fs- se-ff-" id="SE-298f9287-7086-47d4-8958-0ebfc8aec574" style="">이 코드를 실행시키면 multithreading 한 것이 수행 시간이 더 짧다.</span></p><p class="se-text-paragraph se-text-paragraph-align-" id="SE-b0b35a2e-a147-44cd-bc22-60933edcbfe3" style=""><span class="se-fs- se-ff-" id="SE-4fd7d9e0-cf92-4b80-ac4c-7f1146904392" style="">​</span></p><p class="se-text-paragraph se-text-paragraph-align-" id="SE-ff0704d7-8394-4385-abf4-6415b76cf765" style=""><span class="se-fs- se-ff-" id="SE-91ff3a45-30ff-4137-afca-f3acc345a7f2" style="">​</span></p><p class="se-text-paragraph se-text-paragraph-align-" id="SE-4a5591af-99c1-4965-bf3e-e1ab528e654f" style=""><span class="se-fs- se-ff-" id="SE-87b7cbb5-c6b2-4f8e-b142-be67977d469f" style="">mutex (C++11 이상)</span></p><ul class="se-text-list se-text-list-type-bullet-disc"><li class="se-text-list-item"><p class="se-text-paragraph se-text-paragraph-align-" id="SE-2bf62bf2-68b0-44d5-8a9e-c3b32ec252e6" style=""><span class="se-fs- se-ff-" id="SE-93cef98c-6c03-47db-8e0b-01f285e7eda0" style="">2개 이상의 서로 다른 thread가 같은 전역변수를 접근하고 수정 하는 상황을 race condition이라고 한다. 이 race condition이 있으면 보호해야 하는 부분을 critical section/region이라고 한다. mutex는 race condition을 피하기 위해 만들어졌고, std::mutex m1; m1.lock(), m1.unlock()을 사용해 race condition을 피할 수 있다.</span></p></li><li class="se-text-list-item"><p class="se-text-paragraph se-text-paragraph-align-" id="SE-d23d45d3-251f-44b1-8a02-47af9564a105" style=""><span class="se-fs- se-ff-" id="SE-73ed02dd-eda1-4a75-b1b2-ddca6388f8d4" style="">어쨋든 race condition을 피하기 위해서, multithreading에서 쓸 함수에서 어떤 전역변수를 동시에 사용하게 되는 부분을 lock()과 unlock()으로 감싸준다. 그렇게 되면 서로 다른 thread에서 다른 일은 동시에 처리할 수 있는 반면, lock()과 unlock()으로 감싸진 부분은 만약 thread1이 m1.lock()을 실행했다면, thread1에서 m1.lock()이 m1.unlock()으로 풀려야 thread2가 m1.lock()을 수행할 수 있게 된다. 만약 thread1에서 m1.unlock()을 안해준다면 thread2의 critical region은 영원히 접근될 수 없는 것이다.</span></p></li><li class="se-text-list-item"><p class="se-text-paragraph se-text-paragraph-align-" id="SE-14bd2dd6-8dc8-4de1-941c-3fd6ac5c5a93" style=""><span class="se-fs- se-ff-" id="SE-74e56224-3985-47af-9027-349c4176a5c0" style="">mutex의 늘린말은 mutual exclusion이라고 한다.</span></p></li><li class="se-text-list-item"><p class="se-text-paragraph se-text-paragraph-align-" id="SE-399e1186-7355-4753-868e-ec8e1a8bd0c6" style=""><span class="se-fs- se-ff-" id="SE-603d5041-c74c-4833-b1cc-6fad4ce4fb3b" style="">(출처: </span><span class="se-fs- se-ff-" id="SE-66ed31af-e85e-40e1-a53f-654e8eeb3ea6" style=""><a class="se-link" href="https://youtu.be/eZ8yKZo-PGw" target="_blank">https://youtu.be/eZ8yKZo-PGw</a></span><span class="se-fs- se-ff-" id="SE-83a74798-498e-4734-a473-226b585b0ad2" style="">)</span></p></li></ul><!-- } SE-TEXT -->
</div>
</div>
</div>
</div> <div class="se-component se-oglink se-l-text" id="SE-73fa7777-ad8b-4295-b8e7-1093ea420132">
<div class="se-component-content">
<div class="se-section se-section-oglink se-l-text se-section-align-">
<div class="se-module se-module-oglink">
<a class="se-oglink-info" href="https://youtu.be/eZ8yKZo-PGw" target="_blank">
<div class="se-oglink-info-container">
<strong class="se-oglink-title">Mutex In C++ Threading</strong>
<p class="se-oglink-summary">JOIN ME:youtube ► https://www.youtube.com/channel/UCs6sf4iRhhE875T1QjG3wPQ/joinpatreon ► https://www.patreon.com/cppnutsplay list for smart pointers: https:/...</p>
<p class="se-oglink-url">youtu.be</p>
</div>
</a>
</div>
</div>
</div>
<script class="__se_module_data" data-module='{"type":"v2_oglink", "id" :"SE-73fa7777-ad8b-4295-b8e7-1093ea420132", "data" : {"link" : "https://youtu.be/eZ8yKZo-PGw", "isVideo" : "false", "thumbnail" : "https://dthumb-phinf.pstatic.net/?src=%22https%3A%2F%2Fi.ytimg.com%2Fvi%2FeZ8yKZo-PGw%2Fmaxresdefault.jpg%22&amp;type=ff120"}}' type="text/data"></script>
</div> <div class="se-component se-text se-l-default" id="SE-6a0e5f17-1601-4a3d-8914-5c23731e5591">
<div class="se-component-content">
<div class="se-section se-section-text se-l-default">
<div class="se-module se-module-text">
<!-- SE-TEXT { --><p class="se-text-paragraph se-text-paragraph-align-" id="SE-85fa9f58-0675-4cdd-9026-4da5df878342" style=""><span class="se-fs- se-ff-" id="SE-2691d6f9-f17b-490a-ae8e-5a3b0d84d54f" style="">​</span></p><p class="se-text-paragraph se-text-paragraph-align-" id="SE-d89ef585-e53b-4745-b364-ca8bfa22b073" style=""><span class="se-fs- se-ff-" id="SE-abece21a-4077-4e2a-b79f-05f12f50642b" style="">thread (C++11 이상)</span></p><ul class="se-text-list se-text-list-type-bullet-disc"><li class="se-text-list-item"><p class="se-text-paragraph se-text-paragraph-align-" id="SE-f8590f1f-b3cd-4f80-8a7b-ca0c81a0ff57" style=""><span class="se-fs- se-ff-" id="SE-4eec872b-eb37-4285-80d3-5ee29322f21d" style="">multithreading을 통해 parallelism을 구현하기 위해 thread를 사용한다.  thread는 OS가 배정해준다.</span></p></li><li class="se-text-list-item"><p class="se-text-paragraph se-text-paragraph-align-" id="SE-1240c182-1938-4026-ac06-7dbb95481a62" style=""><span class="se-fs- se-ff-" id="SE-e0aa62d5-ed40-4a03-bc46-372ddea9c40a" style="">thread를 사용하는 방법은 1. function pointer 2. lambda function 3. functor (function object) 4. member function, 5. static member function 이 있다.</span></p></li><li class="se-text-list-item"><p class="se-text-paragraph se-text-paragraph-align-" id="SE-0601c11a-0231-4685-b394-93a0a9685f49" style=""><span class="se-fs- se-ff-" id="SE-5c45a336-bf08-48b9-bf97-2731504550f4" style="">1. function pointer로 사용하는 방법은, std::thread t1(함수 이름, 함수에 들어가야하는 인수1, 인수2,...); 형식으로 사용할 수 있다. 4. member function을 사용하는 방법은 class Base 안에 public 함수 run(int x)가 있다고 하면, Base obj; std::thread t1(&amp;Base::run, &amp;obj, 10); 형식으로, 함수의 주소, class 객체의 주소, 파라미터 순으로 입력한다.</span></p></li><li class="se-text-list-item"><p class="se-text-paragraph se-text-paragraph-align-" id="SE-b3c89730-db64-4395-b317-dbda5a0f5065" style=""><span class="se-fs- se-ff-" id="SE-9e96942c-d12d-4bc6-b94e-6f1f7c5b50b8" style="">std::thread.join()은 thread가 끝날때까지 기다리는 것이다. std::thread t1; ...; t1.join()을 하면 그 다음 line은 thread t1이 끝나야 수행되는 것이다. 여기서 주의해야 할 점은 만약 multiple thread를 동시에 만든다면, 무엇이 먼저 시작할지 모른다. 그러나 join은 t1.join(); t2.join(); 을 하면 t1이 끝나기를 기다리고 t2가 끝나기를 기다린다.</span></p></li><li class="se-text-list-item"><p class="se-text-paragraph se-text-paragraph-align-" id="SE-5e702168-3a85-451d-8d83-9b7d0f7c148c" style=""><span class="se-fs- se-ff-" id="SE-537bbc82-d050-49de-9cca-7171b74d6006" style="">특히 double join은 run-time으로 프로그램을 terminate 시킨다. 따라서 if(t1.joinable()) t1.join();처럼 joinable() 함수를 통해 join을 실행시켜주는 것이 좋다.</span></p></li><li class="se-text-list-item"><p class="se-text-paragraph se-text-paragraph-align-" id="SE-f76ea937-9ffb-4f4b-84a6-6ac72f0d9842" style=""><span class="se-fs- se-ff-" id="SE-6995264a-5c82-4f11-813c-081d04a075a1" style="">join은 "wait for this thread to complete"인 반면 detach는 "detach this thread form this parent(main) thread"이다. t1.detach()하면 이 thread를 기다리고 싶지 않다. 이 thread가 실행되는지 관심이 없다.</span></p></li><li class="se-text-list-item"><p class="se-text-paragraph se-text-paragraph-align-" id="SE-4e617bc4-a419-4949-81d1-4913462b2441" style=""><span class="se-fs- se-ff-" id="SE-0750c08e-393a-4c45-90a2-78df78cab656" style="">따라서 만약 어떤 thread를 detach했는데 main 함수가 return하게 되었다면, thread가 모두 실행되지 않고 프로그램이 끝난다. 이때 detached thread의 execution은 suspended 된다고 한다. 즉 detach의 경우 부모 thread가 return되서 없어지면 자식 thread는 실행에 상관없이 다 중단된다.</span></p></li><li class="se-text-list-item"><p class="se-text-paragraph se-text-paragraph-align-" id="SE-334ca269-81ca-425b-b5fb-2baecbbc193d" style=""><span class="se-fs- se-ff-" id="SE-4a6159d5-99d1-49cf-ae33-b335db785580" style="">또한 join() 또는 detach()는 모든 thread에 대해서 반드시 실행되어야 한다. 그 이유는 join() 또는 detach()가 되지 않으면 thread object가 아직 joinable한지 확인해 보고, joinable하다면 thread object의 destructor가 terminate 시키기 때문이다.</span></p></li><li class="se-text-list-item"><p class="se-text-paragraph se-text-paragraph-align-" id="SE-5e0800e3-c8ed-4bcb-9f8d-cf28bf725ae9" style=""><span class="se-fs- se-ff-" id="SE-99efc1d3-58e7-45c5-9622-a7c3cd23fcc7" style="">(출처: </span><span class="se-fs- se-ff-" id="SE-c3aecfd6-bb41-4e69-b994-6171a4548fda" style=""><a class="se-link" href="https://youtu.be/TPVH_coGAQs" target="_blank">https://youtu.be/TPVH_coGAQs</a></span><span class="se-fs- se-ff-" id="SE-77e57efe-bf78-4174-abb9-d150ba777449" style="">)</span></p></li></ul><p class="se-text-paragraph se-text-paragraph-align-" id="SE-bcca7f50-2861-4daf-8262-1ff399e6b608" style=""><span class="se-fs- se-ff-" id="SE-a787931f-16c7-4153-a9f3-3fa6b13eb154" style="">​</span></p><p class="se-text-paragraph se-text-paragraph-align-" id="SE-e6388f9d-4220-4d7f-a602-d380d2b7bdc6" style=""><span class="se-fs- se-ff-" id="SE-02cdf5ed-8db2-4276-91cb-7914a2fe19d4" style="">​</span></p><p class="se-text-paragraph se-text-paragraph-align-" id="SE-6bcab7f0-f797-4ca8-932c-1c47fa53a8ea" style=""><span class="se-fs- se-ff-" id="SE-da963b92-337a-481d-9909-1a3a9b3fee54" style="">RAII</span></p><ul class="se-text-list se-text-list-type-bullet-disc"><li class="se-text-list-item"><p class="se-text-paragraph se-text-paragraph-align-" id="SE-24d7d269-303f-4980-bf62-67e12b4d022d" style=""><span class="se-fs- se-ff-" id="SE-ed705c65-1d39-4aa7-bb85-83f02591922e" style="">resource aquisition is initialization의 줄임말이다.</span></p></li><li class="se-text-list-item"><p class="se-text-paragraph se-text-paragraph-align-" id="SE-58059343-84d0-4274-be94-94598675a5a4" style=""><span class="se-fs- se-ff-" id="SE-ad246285-a8d2-4880-aea8-0f914cfe8b93" style="">resource의 성질은 1. 사용되기 전에 습득해야 하는 것이고, 2. 보통 자원이 limited된 것이다. resource의 예로 heap memory(dynamic allocation), files, mutexes 등이 있다.</span></p></li><li class="se-text-list-item"><p class="se-text-paragraph se-text-paragraph-align-" id="SE-6e093e0c-7deb-40f9-a0b4-3b0020ad426d" style=""><span class="se-fs- se-ff-" id="SE-70ea9a10-ddfe-43fb-9393-3fb8ae32c140" style="">그렇다면 RAII의 결과는, resource lifetime에 대해 더이상 생각할 필요가 없고, object lifetime에 대해서만 신경써면 된다. 따라서 new, delete, free, explicit하게 mutex를 lock/unlock하지 않는다.</span></p></li><li class="se-text-list-item"><p class="se-text-paragraph se-text-paragraph-align-" id="SE-4278dffe-4d69-46f1-a026-cc3cdb55f9a4" style=""><span class="se-fs- se-ff-" id="SE-b7902bfe-d3ce-48e7-8998-a971356ea5d6" style="">왜 new, delete를 절대 쓰지 않아도 되는가? 왜 object lifetime만 신경써도 되는가? RAII class는 constructor에서 resource aquire를 하고, destructor에서 resource release를 해주기 때문이다. 대표적인 RAII class로는 std::vector, std::string, std::unique_ptr, std::shared_ptr, std::lock_gaurd 등이 있다. 이런 것들을 사용하면 프로그램이 안정적이게 되는 것이다. 수동으로 lock/unlock을 안해도 되는 것이다.</span></p></li><li class="se-text-list-item"><p class="se-text-paragraph se-text-paragraph-align-" id="SE-7187b2ae-e203-4705-8555-21aac5b04bd2" style=""><span class="se-fs- se-ff-" id="SE-aa95e6b6-1b03-464e-a3a1-3f4e9c027ad1" style="">(출처: </span><span class="se-fs- se-ff-" id="SE-5d7e49ac-3cd6-462c-8d78-a4c3c1a30218" style=""><a class="se-link" href="https://youtu.be/q6dVKMgeEkk" target="_blank">https://youtu.be/q6dVKMgeEkk</a></span><span class="se-fs- se-ff-" id="SE-f3dcf2fc-e793-4ba4-a630-a35e336982d5" style="">)</span></p></li></ul><p class="se-text-paragraph se-text-paragraph-align-" id="SE-d8ef2284-cc54-4e4d-93a6-e986bcb2c6aa" style=""><span class="se-fs- se-ff-" id="SE-616f17e9-0821-4bfb-94f1-1e9edf493451" style="">​</span></p><!-- } SE-TEXT -->
</div>
</div>
</div>
</div> </div>
</div>
</div>