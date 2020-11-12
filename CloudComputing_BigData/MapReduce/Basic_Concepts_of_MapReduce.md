# BASIC CONCEPTS OF MAPREDUCE

### Hadoop Project

- **MapReduce** 패러다임을 사용하는 대용량 데이터의 분석, 변환을 위한 **프레임워크**와 **분산파일시스템**을 제공
  - 데이터만 분할할 뿐 아니라 computation까지 분산 저장, 처리
  - **병렬적으로 데이터와 최대한 가깝게 처리**
- **일반적인 서버를 추가하는 것만으로도 컴퓨팅 용량, 스토리지 용량 및 IO 대역폭을 확장**



### MapReduce Paper

```
Abstract

MapReduce is a programming model and an associated implementation for
processing and generating large data sets. Users specify a map function that
processes a key/value pair to generate a set of intermediate key/value pairs, and a
reduce function that merges all intermediate values associated with the same
intermediate key. Many real world tasks are expressible in this model, as shown in the
paper.
...
```

