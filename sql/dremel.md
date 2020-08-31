### Dremel : Interactive Analysis of Web-Scale Datasets
#### Abstract
- Dremel은 아래와 같은 특성을 가지며 read-only nested 데이터 분석을 위한 query system이다
	- scalable
	- interactive
	- ad-hoc query
- multi-level execution tree들과 columnar 데이터 레이아웃을 결합하여 trillion(조) 단위 테이블들을 수 초이내 aggregation가능
- 이 논문에서는 아래 2가지를 살펴봄
	- architecture of Dremel
	- implementation of Dremel
	- how it complements MR-based Computing(보완)
#### 1. Introduction
- 이 paper는 보편화된 공유된 머신들의 클러스터들을 통해서 매우 큰 데이터셋의 상호 호환적인 분석을 지원하는 Dremel라는 시스템에대해 설명 
- ```situ``` nested 데이터셋에 대해 작동가능 
- ```situ```는 분산 파일시스템(GFS) 그리고 storage layer(BigTable) 같이 데이터를 in-place(메모리상)에 접근 가능한 능력을 의미
- MR을 대체하기 위한 용도는 아님
- 빠르게 큰 computation을 prototype하거나 MR파이프라인의 결과를  결합하는 혀앹
- Google에서는 2006년 이후 Dremel은 운영 환경에서 쓰였는데 다음과 같은 상황에 쓰임
	- Anaylsis of crawled web documents
	- Tracking install data for application on Android Market
	- Crash reporting for Google products 
	- OCR results from Google Books
	- Spam analysis
	- Debuggin for ma tiles on Google Maps
	- Tablet migrations in managed Bigtable instances
	- Results of test run on Google 's distributed build system
	- Disk I/O statistics for hundereds of thousands of disks
	- Resource monitoring for jobs ru nin Goolge's data centers
	- Symbols and dependencies in Google's codebase
- Dremel은 ```web으로부터 아이디어``` 그리고 ```parellel DBMS```로부터 영감을 받아 만들어짐
	1. 그것의 architecture는 distributed search engine에서 사용하는 serving tree로부터 concept을 빌려옴
		- web search 요청과 같이, query는 tree로 push down 그리고 각가의 step에 rewritten 됨
		- query의 결과는 트리의 가장 낮은 레벨로부터 받은 응답들을 취합(aggregating)하여 구성
	2. Dremel은 high-level SQL과 같은 언어로 express그리고 ad-hoc쿼리를 제공
		- Pig 및 Hive와 달리 ``` MR job으로 변환없이 쿼리를 실행```
	3. Dremel은 column-stripped storage표현방식을 사용
		- 이 방식은 secondary storage로부터 데이터를 덜 읽음
		- cheaper compression(비용이 더싼압축)덕분에 CPU비용을 줄임
	-  Column store들은 relational data을 위해 채택이 되었지만, nested data model에 아이디어가 확장되지는 않았었음
- 이 paper에서는 우리는 다음과 같은 기여를함
	- 우리는 nested data를 위한 참신한 columnar storage format 을 설명. ```nested records들을 column으로 분해하고 다시 재조립하는 algorithm```을 표현(Section 4)
	- Dremel의 SQL그리고 실행에대해 개요를 잡음. ```column-stripped nested data```에대해 둘다 모두 효율적으로 작동하도록 디자인되어야하고, nested record들을 재구성할필요가없음 (Section 5)
	- web 검색 시스템에서 어떻게 트리들이 database processing에서 실행되고, query들을 취합하여 답변할떄 그들의 이점에대해 설명(Section6)
	- 1000-4000node위에 실행된 instance위에서 수조record, 수 TB데이터에대해서 시행된 실험에대해 설명(Section7)
	- 
	- 
