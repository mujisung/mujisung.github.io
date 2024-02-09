---
layout: posts
title: #title
categories: [AI, ]
tag: [SNN, 네이버 블로그]  # tag가 추가됨.
toc: true   # 글 오른쪽에 toc가 나온다.
author_profile: false # 글을 누르면 내 소개가 없어짐. true로 하면 얼굴이 나옴.
sidebar:      # 글을 누르면 목차가 나온다.
  nav: "counts" 
search: true     # false라고 하면 글이 검색이 안된다.
---

#Spiking Neural Networks and Online Learning: An Overview and Perspectives, Lobo et al, 2019 : 네이버 블로그
<div class="wrap_rabbit pcol2 _param(1) _postViewArea223329011505" id="post-view223329011505">
<!-- Rabbit HTML --><div class="se-viewer se-theme-default" lang="ko-KR">
<!-- SE_DOC_HEADER_START -->
<div class="se-component se-documentTitle se-l-default" id="SE-b82e20ca-dc3a-41a4-85ea-ad0a3dd227aa">
<div class="se-component-content">
<div class="se-section se-section-documentTitle se-l-default se-section-align-left">
<!-- -->
<div class="blog2_series">
<a class="pcol2" href="/PostList.naver?blogId=wys000112&amp;categoryNo=26&amp;from=postList" onclick="nclk_v2(this,'pst.category','','');">ML, DL, AI</a>
</div>
<div class="pcol1">
<!-- -->
<div class="se-module se-module-text se-title-text">
<p class="se-text-paragraph se-text-paragraph-align-" id="SE-b24cb59d-4415-4e4a-808e-6212133e8e3b" style=""><span class="se-fs- se-ff-" id="SE-393cc95b-eca0-40d1-b301-1ffdef8f8e9a" style=""><!-- -->Spiking Neural Networks and Online Learning: An Overview and Perspectives, Lobo et al, 2019<!-- --></span></p> </div>
<!-- -->
</div>
<div class="blog2_container">
<span class="writer">
<span class="area_profile"><a class="link" href="https://blog.naver.com/wys000112" onclick="nclk_v2(this,'pst.profile','','');" target="_top"><img alt="프로파일" class="img" src="https://blogpfthumb-phinf.pstatic.net/MjAyMjA1MjVfMTA0/MDAxNjUzNDcxMTU4NTkw.MKx5XZzKhkVnSwLw5O1NM-J45hdDNIrADB_V9VVQBOAg.OkL09v5VWJCO9xIBu4VTEzVASngUXGDvkf4D_exCZsEg.PNG.wys000112/%EB%AC%B4%EC%A7%80%EC%84%B1.png/%25EB%25AC%25B4%25EC%25A7%2580%25EC%2584%25B1.png?type=s1"/></a></span>
<span class="nick"><a class="link pcol2" href="https://blog.naver.com/wys000112" onclick="nclk_v2(this,'pst.username','','');" target="_top">무지성</a></span>
</span>
<i class="dot"> ・ </i>
<span class="se_publishDate pcol2">2024. 1. 21. 11:48</span>
</div>
<div class="blog2_post_function">
<a class="url pcol2 _setClipboard _returnFalse _se3copybtn _transPosition" href="#" id="copyBtn_223329011505" style="cursor:pointer;" title="https://blog.naver.com/wys000112/223329011505">URL 복사</a>
<a class="btn_buddy btn_addbuddy pcol2 _buddy_popup_btn _returnFalse" href="#" onclick="nclk_v2(this,'pst.addnei','','');"><i class="ico"></i> 이웃추가<i class="aline"></i></a>
<div class="overflow_menu">
<a area-expanded="false" area-haspopup="true" class="btn_overflow_menu _open_overflowmenu pcol2 _param(223329011505) _returnFalse" href="#" role="button"><span class="blind">본문 기타 기능</span></a>
<div area-hidden="true" class="lyr_overflow_menu" id="overflowmenu-223329011505">
<a class="naver-splugin btn_splugin share _title_share" data-canonical-url="https://blog.naver.com/wys000112/223329011505" data-likecontentsid="wys000112_223329011505" data-likeserviceid="BLOG" data-logdomain="https://proxy.blog.naver.com/spi/v1/api/shareLog" data-me-display="off" data-oninitialize="splugin_oninitialize(1);" data-option="{baseElement:'_title_spiButton', layerPosition:'outside-bottom', align:'right', marginLeft:0, marginTop:4}" data-style="unity" data-url="https://blog.naver.com/wys000112/223329011505" href="#" id="_title_spiButton" onclick="return false;">
                   공유하기
                <span class="ico_share _title_share_icon"></span>
</a>
<a class="_report _param(https://srp2.naver.com/report?svc=BLG&amp;exit=close&amp;ctype=AA01&amp;cwriterenc=5Qy3DNgaMZYtD7p9G2Kd0GcyWVtY%2FVSSyBe1hTGLhKY%3D&amp;ctitle=Spiking%20Neural%20Networks%20and%20Online%20Learning%3A%20An%20Overview%20and%20Perspectives%2C%20Lobo%20et%20al%2C%202019&amp;cwriter=wys0*****&amp;dark=disable&amp;memtype=Y&amp;env=pc&amp;cnickname=wys0*****&amp;vsvc=BLG&amp;cid=wys000112%40%4051896191%40%40mylog%40%40223329011505) _returnFalse" href="#">신고하기<span class="ico_report"></span></a>
</div>
</div>
<input alt="url" class="copyTargetUrl" style="display:none;" title="URL 복사" type="text" value="https://blog.naver.com/wys000112/223329011505"/>
</div>
<!-- -->
</div>
</div>
</div>
<!-- B2C 상품 -->
<!-- _BLOG_CONTENTS_HEADER_TAIL -->
<!-- SE_DOC_HEADER_END -->
<div class="se-main-container">
<div class="se-component se-text se-l-default" id="SE-4bcb1472-3500-418e-aebb-106ff5d05171">
<div class="se-component-content">
<div class="se-section se-section-text se-l-default">
<div class="se-module se-module-text">
<!-- SE-TEXT { --><p class="se-text-paragraph se-text-paragraph-align-" id="SE-ffe084dc-d546-4db5-acc4-50eef96d67a0" style=""><span class="se-fs- se-ff-" id="SE-5444b4b3-6c34-4237-9106-5c73ab53fe34" style="">​</span></p><p class="se-text-paragraph se-text-paragraph-align-" id="SE-74ca6430-0c92-4fa1-8368-c2fe8438288d" style=""><span class="se-fs- se-ff-" id="SE-ab85e12b-ec6d-4d6e-a7d9-cd1b8a482a55" style="">읽은 후 나의 생각: </span></p><ul class="se-text-list se-text-list-type-bullet-disc"><li class="se-text-list-item"><p class="se-text-paragraph se-text-paragraph-align-" id="SE-c68310e3-9708-4342-afab-e3f50c4a311c" style=""><span class="se-fs- se-ff-" id="SE-f918acd5-4a78-43c3-a341-7f47f2e1c707" style="">결국 backpropagation이 우리가 무언가를 배우는 방식. error를 계속 만들어나감으로써, 뒤의 뉴런에도 영향을 끼칠 수 있도록. </span></p></li><li class="se-text-list-item"><p class="se-text-paragraph se-text-paragraph-align-" id="SE-20deb27f-be1f-4fb2-ad76-be85ee00d1be" style=""><span class="se-fs- se-ff-" id="SE-171bd415-abb7-42bb-b2e4-a7defe8f5113" style="">lifelong ML: 다른 task, 다른 data distribution에서도 captured 지식을 유연하게 update할 수 있도록.</span></p></li></ul><!-- } SE-TEXT -->
</div>
</div>
</div>
</div> </div>
</div>
</div>