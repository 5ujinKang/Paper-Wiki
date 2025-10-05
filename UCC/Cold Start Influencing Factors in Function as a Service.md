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

## 목적

- FaaS에서 cold start에 영향을 미치는 요인 조사
- 클라우드 Functions에서의 cold start를 벤치마킹



## 문제상황 = 연구가 필요한 이유

- Cold Start (최초 실행)는 가상화 기술(Serverless)들이라면 꼭 겪는 문제
    
    Cold start refers to the state our function when serving a particular invocation request. A serverless function is served by one or multiple micro-containers. When a request comes in, our function will check whether there is a container already running to serve the invocation. When an idle container is already available, we call it a “warm” container. If there isn’t a container readily available, the function will spin up a new one and this is what we call a “cold start”.
    
    When a function in a cold state is invoked, the request will take additional time to be completed, because there’s a latency in starting up a new container. That’s the problem with cold starts: they make our application respond slower. In the “instant-age” of the 21st century, this could be a big problem.
    
- Cold Start in FaaS & 문제
    - 클라우드 function은 컨테이너 안에서 실행됨
    - 클라우드 function 첫 실행시 컨테이너 cold start 필요 : 컨테이너가 먼저 시작이 되어야 function이 그 안에서 실행될 수 있으니까
    - 그래서 성능을 위해서 FaaS 제공자는 컨테이너를 바로 종료시키지 않음
        - 놔두고 다음 실행을 warm execution 환경에서 시키려고 (최초실행 필요 X)
    - 근데 idling 피하기 & on-demand 스케일링을 하려다보니 더 많은 cold start를 할 수 밖에 없음
        - avoidance of idling : idling 피하기
            - idle :  runs slowly but does not do any work
        - scaling on demand : on-demand 방식으로 스케일링 (그때그때 필요할때마다 크기 조정)
    - 기존에는 클라우드 function을 pinging 하는 걸로 어느정도 이 문제를 피해갔지만, 이 방식은 FaaS의 the scale to zero 원칙에 위배됨 ⇒ 새로운 방식 필요
        - ping : query (another computer on a network) to determine whether there is a connection to it. = 상태 전달
        - 논문에 따르면 Ping을 보내는 방식은 “일정 개수의 컨테이너가 항상 작동된다”는 것만 보장함
        - [Scale to Zero - OpenFaaS](https://docs.openfaas.com/openfaas-pro/scale-to-zero/) : 용량이 0으로 스케일 된 function은 CPU나 메모리를 사용하면 안됨⇒ ping을 보내면 사용하게 됨.
     

## 가설설정

1. **Programming Language** 
    - interpreted language(ex. Java Script)보다 compiled language(ex. Java, C#)가 더 많은 cold Start overhead를 발생시킬것이다.
        - environment overhead 때문에
        (ex. JAVA로 쓰여진 cloud function ⇒ 실행되기 전에 Java Virtual Machine 이 먼저 세팅되어있어야함)
2. **Deployment Package Size**
    - deployment Package Size가 증가하면 cold start overhead도 증가할 것이다.
        - Deployment package : A deployment package is a way to enable users to quickly deploy Java or .NET applications to any environment using CloudBees CD/RO. A deployment package is a .zip file that is made up of a manifest file and one or more components that make up an application.
        
3. **Memory/CPU Setting**
    - 자원이 늘어나면 cold Start overhead는 감소할 것이다.
        - 컨테이너가 좀 더 빨리 로드되고 셋팅될 수 있기 때문에
4. **Number of Dependencies**
    - dependencies의 양과 크기가 증가하면 cold start overhead는 증가할 것이다.
        - dependencies는 첫 실행전에 로드 되어야함 ⇒ 계속 사용될 수 있도록
        - dependencies를 로딩해오는데는 시작이 걸림
5. **Concurrency Level**
    - concurrency level은 cold start overhead에도, 실행시간에도 아무런 영향을 주지 않을것이다.
        - Concurrency Level : concurrent requests의 수 = 시작된 컨테이너의 수
        - 이유 : function들은 별도의 컨테이너에서 서로 독립적으로 시작됨 ⇒ 컨테이너 수가 늘어나든 줄어나든 function자체의 실행과는 연관없음
6. **Prior Executions**
    - 이전 실행과 cold start overhead는 관련이 없을 것이다. (cold start자체가 최초실행이기 때문에)
        - PaaS와 달리 Avoidance of idling은 FaaS의 엄청난 발전이지만, 이것 때문에 사용되지 않는 컨테이너는 바로 종료시켜버림
        - 다음번 실행때는 새로운 컨테이너를 시작시켜야함
7. **Container Shutdown**
    - 어떤 컨테이너가 셧다운된채로 머물러 있는 시간은 이전의 실행 횟수에 달려있을것이다.
        - 비용절약과 사용자 만족 때문에, 플랫폼들은 자주 쓰이는 cloud function들을 알고리즘을 통해 골라내게할 수 도 있음
        - FaaS 패러다임에서는 실행은 서로 독립적이고 컨테이너의 생명주기에 영향을 미쳐서는 안된다고 나옴.


## 실험환경

- 가설 선택 :  7개 중에 3개로만 진행
    - Programming Language
    - Deployment Package Size
    - Memory/CPU Setting
    
- 플랫폼 및 언어선택
    - 언어 : JAVA & JS
        - 대표적인 compiled & interpreted language
        - 상업적으로도 오픈소스 커뮤니티에서도 많이 사용되는 언어들임
    - 플랫폼 : AWS Lambda & Microsoft Azure Functions
        - 자바를 지원하는 유일한 플랫폼들임
        - Azure Cloud Function은 메모리와 CPU에 대한 자동 스케일링을 제공 ⇒ Azure로 Memory/CPU setting가설을 검증할 수는 없음 ⇒ 이건 AWS로만 진행
        - AWS는 별다른 언급없으면 메모리 크기 256MB인 cloud function 사용
- 알고리즘 선택 : 피보나치 재귀버전으로 n번째 피보나치 값을 계산
    - 세부
        - 재귀적 피보나치는 compute bound ⇒ 실행시간이 기기의 처리속도에 따라 결정
            - compute bound (=**CPU-bound**) :  when the time for it to complete a task is determined principally by the speed of the [central processor](https://en.wikipedia.org/wiki/Central_processing_unit) processor utilization is high, perhaps at 100% usage for many seconds or minutes.
        - 2개의 재귀 호출을 가진 재귀 알고리즘 ⇒ 트리는 기하급수적으로 증가
        - 계산이 memory bound이진 않음.
            - 한 단계에 대한 스택 평가가 다 끝나기 전까지는 다음 계산이 시작하지 않기 때문
            - ex. fib(n-1)이 다 평가되고 나서 fib(n-2)가 호출됨
            - 결과적으로 시간복잡도 = O(2^n) / 공간복잡도 : O(n)
        - 시간복잡도를 어느정도 예측가능 ⇒ 안정적인 결과 도출 가능
        - 메모리 사용량이 적기때문에 실험에 사용하기에 적절 ⇒ 어떤 메모리 환경에서도 function을 benchmark할 수 있음.

- 핵심
    - FaaS 플랫폼에 API gateway 셋팅
    
    ⇒ function 실행을 위해 로컬 REST 호출을 host machine에서 보낼 수 있도록
    
    - 로컬에서 측정했을때의 function 시작시간, 끝난 시간을 클라이언트측에서 logging
    - 플랫폼 상에서의 time stamp와 비교
    
    - 세부단계 (클라이언트 : host machine of the FaaS user)
        1. 클라이언트 단) 시작시간 time stamp 저장
        2. 클라이언트 → 플랫폼 ) REST 호출 / 플랫폼의 API gateway에 요청을 보냄
        3. 플랫폼) 컨테이너 생성 or 재사용
        4. 플랫폼 ) cloud function 실행
        5. 플랫폼) 미들웨어가 function 시작시간, 종료시간을 log함
        6. 플랫폼 ) 연산결과는 응답결과로써 API gateway endpoint로 전달
        7. 플랫폼 → 클라이언트 ) 결과를 클라이언트에 전달
        8. 클라이언트 ) REST 호출 종료 && 로컬에서의 종료 타임스탬프 기록
    - 클라이언트 단에서의 기록과 플랫폼 단에서의 실행시간 차이,  warm start/ cold start 에서의 실행시간 차이를 알 수 있음
- 각 cloud function은 550번 호출됨 ⇒ 275쌍의 cold execution, warm execution을 하기위해
    - HTTP 상태코드 500번이 return 되는 경우 = 서버오류 나는 경우는 제외하고 평가

## 실험결과

- 전반적인 분석

## 의문점

---

- 참고
    - warm start : 기존에 존재하는 컨테이너 그냥 이어서 사용
    - Cold Start 에 영향을 미치는 요소
    : 프로그래밍 언어, 플랫폼, 메모리 사이즈 for cloud function, 사용되는 artifact 용량
        - Artifacts might be [databases](https://www.techtarget.com/searchdatamanagement/definition/database)
        , data models, printed documents or [scripts](https://www.techtarget.com/whatis/definition/script)
    - Java Script는 작고, stateless한 cloud function에 적합
    - Deployment package : A deployment package is a way to enable users to quickly deploy Java or .NET applications to any environment using CloudBees CD/RO. A deployment package is a .zip file that is made up of a manifest file and one or more components that make up an application.
    - compute bound (=**CPU-bound**) :  when the time for it to complete a task is determined principally by the speed of the [central processor](https://en.wikipedia.org/wiki/Central_processing_unit) processor utilization is high, perhaps at 100% usage for many seconds or minutes.
    - **Memory bound**
     refers to a situation in which the time to complete a given [computational problem](https://en.wikipedia.org/wiki/Computational_problem)
     is decided primarily by the amount of free [memory](https://en.wikipedia.org/wiki/Computer_memory)
     required to hold the working [data](https://en.wikipedia.org/wiki/Data)
    - memory complexity = space complexity
    - [Serverless: Develop & Monitor Apps On AWS Lambda](https://www.serverless.com/) : Easily develop and monitor auto-scaling applications on AWS Lambda, API Gateway, DynamoDB, etc.,
    - provisioning : **사용자의 요구에 맞게 시스템 자원을 할당, 배치, 배포해 두었다가 필요 시 시스템을 즉시 사용할 수 있는 상태로 미리 준비해 두는 것을 말한다**
    
