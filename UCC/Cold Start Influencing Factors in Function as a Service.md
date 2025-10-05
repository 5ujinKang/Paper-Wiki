# Cold Start Influencing Factors in Function as a Service

[Google Slides Link](https://docs.google.com/presentation/d/1AGKK0Hax0hDF3EUPpMPeYdThUCDn-2i4M09xkgj1vtU/edit#slide=id.g13bd5a14bf3_0_5)

## 핵심어

- **Serverless Computing**, **Function as a Service (FaaS)**, **Cloud Functions**, **Cold Start**, **Benchmarking**

<details>
<summary>Serverless Computing</summary>

### Serverless vs FaaS
- **Serverless**: 일반적이고 추상적인 개념  
  - A cloud-native development model that allows developers to build and run applications **without managing servers**.  
  - Servers exist but are abstracted from app development.
- **FaaS**: Event-driven cloud function

</details>

<details>
<summary>Function as a Service (FaaS)</summary>

![Cloud Architecture](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c61f0182-c17f-460a-aedb-c23794ea2cd8/Untitled.png)  
> 파란색 부분이 사용자가 구축할 필요가 없는 부분 = 클라우드에서 제공

- 급속도로 발전 중인 클라우드 패러다임  
- 클라우드 컴퓨팅 서비스의 일종  
- 사용자가 애플리케이션 기능을 개발, 실행, 관리할 수 있도록 플랫폼 제공 (인프라 구축/유지 불필요)  
- **PaaS 대비 99.95% 비용 절감** (by avoidance of idling)

</details>

<details>
<summary>Cloud Functions</summary>

- 서버리스 환경에서 제공되는 개별 기능 단위

</details>

<details>
<summary>Cold Start</summary>

- 최초 실행 시 발생하는 지연  
- 대비: **Warm start** – 이미 시작된 환경에서 이어서 실행

</details>

<details>
<summary>Benchmarking</summary>

- 대상을 비교 또는 평가할 수 있는 기준 또는 지표

</details>

