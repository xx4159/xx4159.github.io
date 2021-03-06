---
layout: post
title: Google Analytics
description: Understanding Google Analytics UTM
tags: [ga]
---
# Google Analytics
- 유입 트래킹 분석 툴
- 구글이 관리하는 트래킹코드로 작동

## Dimensions(campaign-level / ad-level)
- 유입경로를 제공하기 위한 카테고리
	- **Source**: 그 사이트로 가라고 알려준 놈.(Who)(campaign/ad-level)
		+ direct: url을 직접 치고 들어온 경우 
		+ google, facebook, bing...
	- **Medium**: 그 Source 놈이 제공한 매체(수단)(How)(campaign/ad-level), 대게 자동으로 셋팅 되지 않아 organic 으로 적용됨.
		+ none: url을 직접 치고 들어온 경우 (Source: direct)
		+ referral: 추천 링크로 유입(default)
		+ cpc: 돈을 지불하고 한 광고 클릭 후 유입된 경우. paid traffic. cpc(cost per click),paid,ad
		+ (not set): ga도 모름.
		+ organic: 검색엔진(e.g., 구글) 검색 결과 통해서 유입
		+ email
		+ social
		+ video: 영상의 링크로 유입
	- **Campaign**: 홍보한 이름(campaign/ad-level)
	- **Term**: (ad-level)
	- **Content**: (ad-level)


## UTM parameter
- url에 트래킹 파라미터를 추가하는 방식으로 정보를 제공함.
- 파라미터: `utm_source` `utm_medium` `utm_campaign` `utm_term` `utm_content`
- utm 파라미터를 달고 오지 않으면 유입 경로를 알 수 없다? Google AdWords 가 자동 태그 추가 제공?
- [Campaign URL Builder](https://ga-dev-tools.appspot.com/campaign-url-builder/): 일관된 파라미터를 적용하기 URL을 만들어주는 툴
- 가이드라인 작성하면 좋겠다.


## 참고 링크
- [Google Analytics UTM tagging 
best practices](https://funnel.io/resources/google-analytics-utm-tagging-best-practices)
- [UTM Parameter 를 달아 링크 유입 분석하기](https://milooy.wordpress.com/2015/05/20/google-analytics-with-utm-parameter/)
