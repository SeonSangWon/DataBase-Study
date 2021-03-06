2020.01.20

# Reorg & Partitioning

## Reorg 란 무엇인가 ?
 - 일종의 디스크 모음, DB 의 데이터블록을 효율적으로 관리하여 최적의 성능을 만들어주는 방법
   + ALTER TABLE MOVE 명령어를 이용해서 빈 공간이 없도록 사용했지만 Oracle 10g 부터 Reorg 기술 지원을 통해 단편화를 해소
   * DB 의 성능을 저하시키는 문제는 HWM(고수위, High Water Mark)
   
## Partitioning 란 무엇인가 ?
 - 큰 TABLE 이나 INDEX를 관리하기 쉬운 단위로 분리하는 방법
 - 보통 연속적인 숫자나 날짜를 기준으로 Partitioning 한다
 - 손쉬운 관리 기법 제공에 따른 관리 시간의 단축
 - 우편번호 / 일별 / 월별 / 분기별등의 데이터에 적합한 
 
## Partitioning Benefit
 - 가용성
   + 물리적인 Partitioning 으로 인해 전체 데이터의 훼손 가능성이 줄어들고, 데이터 가용성이 향상된다
 - 관리용이성
   + 큰 TABLE 들을 제거하여 관리를 쉽게 해준다
 - 성능
   + 특정 DML 과 Query 의 성능을 향상시킨다
 - 단점
   + TABLE 간의 JOIN 에 대한 비용이 증가한다
   + TABLE 과 INDEX 를 별도로 Partition 할 수는 없다
   
## HWM(고수위, High Water Mark) 란 무엇인가 ?
 - Extent 확장의 기준, 모든 Segment 에 하나씩 존재
 - 1 번에 5 개의 데이터 블록 단위로 HWM 이동
 - Full Scan 수행 시, HWM 앞의 모든 데이터 블록을 액세스
 - Data 가 적은데, Full Scan 시간이 오래 걸리면 Segment 축소 필요
 
## DELETE 와 TRUNCATE 의 차이점
 - DELETE 는 데이터를 삭제해도 기존에 할당 된 영역 및 HWM 의 위치가 그대로이다
 - TRUNCATE 는 자동 COMMIT
 - TRUNCATE 는 HWM 을 초기화 시킨다
 - TRUNCATE 는 MinExtents 설정 값만큼을 제외하고 남은 Extents 는 모두 할당 해제한다
 - 전체 데이터를 삭제 시, DELETE 보다 TRUNCATE 를 사용하는 것이 성능면에서 유리하다
