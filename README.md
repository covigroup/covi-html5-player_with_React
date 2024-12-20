# Covi HTML5 임베디드 플레이어 리액트 연동 가이드
<details>
<summary>Covi 플레이어 사용을 위해 리액트 라이브러리와 연동하는 방법을 안내합니다.</summary>
<div>
  <ul>
    <p>
    <li>Covi 플레이어의 생성, 적용, 정리</li>
    </ul>
</div>
</details>

<details>
<summary>Covi 플레이어는 <code>VAST 3.0</code>을 지원합니다.</summary>
<div>
  <ul>
    <p>
    <li>Covi 플레이어에 Covi SDK 기능이 포함됨</li>
        <ul>
          <li>Covi SDK 기능 : 이벤트 트래킹 로그 전송, 가시성 체크, VAST 3.0 XML 요청 및 응답결과 파싱</li> <p>
       </ul>
    <li>Covi 플레이어 탑재 기능 : 플레이어 UI-UX, 영상 재생, 영상 정지, 영상 종료, 영상 포커싱 감지, 영상 가시성 감지</li>
  </ul>
</div>
</details>

> `Covi 플레이어` : `Covi HTML5 임베디드 플레이어`의 단축어

<br>

### 목차
  + [React 함수형 컴포넌트 연동](https://github.com/covigroup/covi-html5-player_with_React/tree/main#react-%ED%95%A8%EC%88%98%ED%98%95-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8-%EC%97%B0%EB%8F%99)
    - [Script 태그 동적 삽입](https://github.com/covigroup/covi-html5-player_with_React/tree/main#script-%ED%83%9C%EA%B7%B8-%EB%8F%99%EC%A0%81-%EC%82%BD%EC%9E%85)
      * [`useEffect`로 Script 태그 삽입 로직 처리](https://github.com/covigroup/covi-html5-player_with_React/tree/main#1-useeffect%EB%A1%9C-script-%ED%83%9C%EA%B7%B8-%EC%82%BD%EC%9E%85-%EB%A1%9C%EC%A7%81%EC%9D%84-%EC%B2%98%EB%A6%AC%ED%95%A9%EB%8B%88%EB%8B%A4)
      * [`src/customHooks` 디렉터리에서 `useScript.jsx` 파일 생성](https://github.com/covigroup/covi-html5-player_with_React/tree/main#2-srccustomhooks-%EB%94%94%EB%A0%89%ED%84%B0%EB%A6%AC%EC%97%90%EC%84%9C-usescriptjsx-%ED%8C%8C%EC%9D%BC%EC%9D%84-%EC%83%9D%EC%84%B1%ED%95%A9%EB%8B%88%EB%8B%A4)
     
    - [Covi 플레이어 생성 및 SDK 적용](https://github.com/covigroup/covi-html5-player_with_React/tree/main#covi-%ED%94%8C%EB%A0%88%EC%9D%B4%EC%96%B4-%EC%83%9D%EC%84%B1-%EB%B0%8F-sdk-%EC%A0%81%EC%9A%A9)
      * [`className`이 `covi`인 div element를 생성](https://github.com/covigroup/covi-html5-player_with_React/tree/main#1-classname%EC%9D%B4-covi%EC%9D%B8-div-element%EB%A5%BC-%EC%83%9D%EC%84%B1%ED%95%A9%EB%8B%88%EB%8B%A4)
      * [SDK CDN 정보](https://github.com/covigroup/covi-html5-player_with_React/tree/main#2-sdk-cdn-%EC%A0%95%EB%B3%B4)
      * [COVI Player SDK 적용](https://github.com/covigroup/covi-html5-player_with_React/tree/main#3-classname%EC%9D%B4-covi%EC%9D%B8-div-element%EC%97%90-covi-player-sdk%EB%A5%BC-%EC%A0%81%EC%9A%A9%ED%95%A9%EB%8B%88%EB%8B%A4)
     
    - [Covi 플레이어 정리](https://github.com/covigroup/covi-html5-player_with_React/tree/main#covi-%ED%94%8C%EB%A0%88%EC%9D%B4%EC%96%B4-%EC%A0%95%EB%A6%AC)
      * [함수형 컴포넌트가 언마운트되는 시점에 `disposeCoviplayers` SDK 내장 함수를 사용](https://github.com/covigroup/covi-html5-player_with_React/tree/main#%ED%95%A8%EC%88%98%ED%98%95-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8%EA%B0%80-%EC%96%B8%EB%A7%88%EC%9A%B4%ED%8A%B8%EB%90%98%EB%8A%94-%EC%8B%9C%EC%A0%90%EC%97%90-disposecoviplayers-sdk-%EB%82%B4%EC%9E%A5-%ED%95%A8%EC%88%98%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%A9%EB%8B%88%EB%8B%A4)

    - [SDK 내장 함수 사용](https://github.com/covigroup/covi-html5-player_with_React/tree/main#sdk-%EB%82%B4%EC%9E%A5-%ED%95%A8%EC%88%98-%EC%82%AC%EC%9A%A9)
      * [runCoviPlayer()](https://github.com/covigroup/covi-html5-player_with_React/tree/main#1-runcoviplayer)
      * [disposeCoviplayers()](https://github.com/covigroup/covi-html5-player_with_React/tree/main#2-disposecoviplayers)

  + [이벤트 트래킹 로그 전송 체크](https://github.com/covigroup/covi-html5-player_with_React/tree/main#%EC%9D%B4%EB%B2%A4%ED%8A%B8-%ED%8A%B8%EB%9E%98%ED%82%B9-%EB%A1%9C%EA%B7%B8-%EC%A0%84%EC%86%A1-%EC%B2%B4%ED%81%AC)
    - [이벤트 트래킹(Event Tracking)](https://github.com/covigroup/covi-html5-player_with_React/tree/main#1-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%ED%8A%B8%EB%9E%98%ED%82%B9event-tracking)
    - [COVI SDK가 적용된 비디오 플레이어의 진행 시간에 따른 이벤트 트래킹 체크](https://github.com/covigroup/covi-html5-player_with_React/tree/main#2-1-covi-%ED%94%8C%EB%A0%88%EC%9D%B4%EC%96%B4-%EA%B4%91%EA%B3%A0-%EC%98%81%EC%83%81-%EC%A7%84%ED%96%89-%EC%8B%9C%EA%B0%84%EC%97%90-%EB%94%B0%EB%A5%B8-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%ED%8A%B8%EB%9E%98%ED%82%B9-%EC%B2%B4%ED%81%AC)
    - [클릭을 통해 광고주 랜딩 페이지로 이동했을 때 이벤트 트래킹 체크](https://github.com/covigroup/covi-html5-player_with_React/tree/main#2-2-%ED%81%B4%EB%A6%AD%EC%9D%84-%ED%86%B5%ED%95%B4-%EA%B4%91%EA%B3%A0%EC%A3%BC-%EB%9E%9C%EB%94%A9-%ED%8E%98%EC%9D%B4%EC%A7%80%EB%A1%9C-%EC%9D%B4%EB%8F%99%ED%96%88%EC%9D%84-%EB%95%8C-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%ED%8A%B8%EB%9E%98%ED%82%B9-%EC%B2%B4%ED%81%AC)
    - [이벤트 트래킹 로그별 전송 시점](https://github.com/covigroup/covi-html5-player_with_React/tree/main#3-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%ED%8A%B8%EB%9E%98%ED%82%B9-%EB%A1%9C%EA%B7%B8%EB%B3%84-%EC%A0%84%EC%86%A1-%EC%8B%9C%EC%A0%90)

  + [가시성 측정 체크](https://github.com/covigroup/covi-html5-player_with_React/tree/main#%EA%B0%80%EC%8B%9C%EC%84%B1-%EC%B8%A1%EC%A0%95-%EC%B2%B4%ED%81%AC)
    - [가시성(Viewability)](https://github.com/covigroup/covi-html5-player_with_React/tree/main#1-%EA%B0%80%EC%8B%9C%EC%84%B1viewability)
    - [가시성 측정 프로세스](https://github.com/covigroup/covi-html5-player_with_React/tree/main#2-%EA%B0%80%EC%8B%9C%EC%84%B1-%EC%B8%A1%EC%A0%95-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4)
    - [COVI 플레이어의 가시성 측정이 정상 작동하는지 체크](https://github.com/covigroup/covi-html5-player_with_React#3-covi-%ED%94%8C%EB%A0%88%EC%9D%B4%EC%96%B4%EC%9D%98-%EA%B0%80%EC%8B%9C%EC%84%B1-%EC%B8%A1%EC%A0%95%EC%9D%B4-%EC%A0%95%EC%83%81-%EC%9E%91%EB%8F%99%ED%95%98%EB%8A%94%EC%A7%80-%EC%B2%B4%ED%81%AC%ED%95%98%EA%B8%B0)

  + [참조](https://github.com/covigroup/covi-html5-player_with_React#%EC%B0%B8%EC%A1%B0)

<br>

## React 함수형 컴포넌트 연동

* 함수형 컴포넌트로 작성된 환경에서 **Covi 플레이어를 생성, 적용, 정리하는 예제**를 안내합니다.

* 아래 React 함수형 컴포넌트 연동 예제를 실행하기 위해 `react >= 18.2.0`, `react-dom >= 18.2.0` 설치를 권장합니다.

<br>

## Script 태그 동적 삽입

### 1) `useEffect`로 Script 태그 삽입 로직을 처리합니다.
  - [`useEffect`](https://react.dev/reference/react/useEffect) : 함수형 컴포넌트에서 외부 시스템과의 동기화, 비동기화를 제어할 때 사용하는 리액트 hooks

<br>

### 2) `src/customHooks` 디렉터리에서 [`useScript.jsx`](https://usehooks.com/usescript) 파일을 생성합니다.
  - url 파라미터로 받아온 주소로 스크립트를 생성하고, 생성한 스크립트의 상태를 리턴합니다.

```
import { useState, useEffect } from 'react';

export default function useScript(url) {
    const [status, setStatus] = useState(url ? 'loading' : 'error');

    useEffect(() => {
        if (!url) {
            setStatus('error');
            return;
        }

        const setStateFromEvent = (event) => {
            setStatus(event.type === 'load' ? 'ready' : 'error');
        };

        let script = document.querySelector(`script[src="${url}"]`);

        if (!script) {
            script = document.createElement('script');
            script.src = url;
            script.async = true;
            script.setAttribute('data-status', 'loading');
            document.head.appendChild(script);

            const setAttributeFromEvent = (event) => {
                script.setAttribute('data-status', event.type === 'load' ? 'ready' : 'error');
                setStateFromEvent(event.type);
            };

            script.addEventListener('load', setAttributeFromEvent);
            script.addEventListener('error', setAttributeFromEvent);
        } else {
            setStatus(script.getAttribute('data-status'));
        }

        script.addEventListener('load', setStateFromEvent);
        script.addEventListener('error', setStateFromEvent);

        return () => {
            if (script) {
                script.removeEventListener('load', setStateFromEvent);
                script.removeEventListener('error', setStateFromEvent);
            }
        };
    }, [url]);

    return status;
}
```

<br>

## Covi 플레이어 생성 및 SDK 적용

### 1) `className`이 `'covi'`인 div element를 생성합니다.

```
<div className='covi'></div>;
```

<br>

### 2) SDK 정보
- Covi player SDK URL
```
https://covi-plat-file.beta.covi.co.kr/player/js/coviplayer.js
```

- Publisher 스크립트 URL
```
매체 제휴를 진행하면서 전달받은 Publisher 스크립트 URL 사용 (전달받은 Publisher 스크립트 URL이 없을 시 COVI 개발자에게 문의해주세요)
```

> 위 SDK 스크립트는 개발용입니다. 상용 서비스 전환 시 SDK 스크립트의 URL이 달라지므로 상용 URL을 받아서 사용해야 합니다. 
>
> 개발용 SDK를 적용해서 연동을 마친 후 COVI 매체 제휴 담당자에게 상용 URL 주소를 문의해 주세요.

<br>

### 3) `className`이 `covi`인 div element에 COVI Player SDK를 적용합니다.

- 페이지를 렌더링하는 컴포넌트에서 `coviScriptStatus`와 `publisherScriptStatus`가 `ready`상태일 때 `runCoviplayer` [SDK 내장 함수를 사용](https://github.com/covigroup/covi-html5-player_with_React#sdk-%EB%82%B4%EC%9E%A5-%ED%95%A8%EC%88%98-%EC%82%AC%EC%9A%A9)합니다.
  + `runCoviplayer` : COVI Player 인스턴스를 생성하고 `className`이 `covi`인 div 요소에 SDK를 적용합니다.



```
import { useEffect } from 'react';
import useScript from '../customHooks/useSrcipt';
import { PlayerSample } from '../components/PlayerSample';

const Page = () => {

    const [isCoviSdkFirstLoaded, setIsCoviSdkFirstLoaded] = useState(true);

    // Covi player SDK CDN Script 동적 삽입
    const [coviScriptStatus, publisherScriptStatus] = [useScript('https://covi-plat-file.beta.covi.co.kr/player/js/coviplayer.js'), useScript('전달받은 Publisher 스크립트 URL')];

    useEffect(() => {
        if (coviScriptStatus === 'ready' && publisherScriptStatus === 'ready' && isCoviSdkFirstLoaded === true) {
            setIsCoviSdkFirstLoaded((isCoviSdkFirstLoaded) => false);
        } else if (coviScriptStatus === 'ready' && publisherScriptStatus === 'ready' && isCoviSdkFirstLoaded === false) {
            if (typeof runCoviplayer === 'function') {
                runCoviplayer();
            }
        }
    }, [coviScriptStatus, publisherScriptStatus, isCoviSdkFirstLoaded]);

    return (
        <>
          <div className='covi'></div>;
        </>
    )
}

export default Page;
```

<br>

## Covi 플레이어 정리
### 함수형 컴포넌트가 언마운트되는 시점에 `disposeCoviplayers` [SDK 내장 함수를 사용](https://github.com/covigroup/covi-html5-player_with_React#sdk-%EB%82%B4%EC%9E%A5-%ED%95%A8%EC%88%98-%EC%82%AC%EC%9A%A9)합니다.
  + `disposeCoviplayers` : 모든 COVI Player 인스턴스를 정리(dispose) 합니다.

> <주의> Covi 플레이어 정리를 위해 표준 DOM 제거 방법을 사용하지 마십시오. 
>
> 표준 DOM 제거 방법을 사용하면 SDK 적용하면서 생성된 이벤트 리스너와 Covi Player 인스턴스가 메모리에 남게 됩니다.

```
    useEffect(() => {
        return () => {
            disposeCoviplayers();
        };
    }, []);
```

<br>

## SDK 내장 함수 사용
  - 전역 window 객체에서 Covi SDK 내장 함수 runCoviPlayer, disposeCoviplayers 함수를 가져와서 사용할 수 있습니다.
```
const { runCoviPlayer, disposeCoviplayers } = window
```

### 1) runCoviPlayer()
COVI Player 인스턴스를 생성하고 로드합니다.

### 2) disposeCoviplayers()
`disposeCoviplayers` : 모든 COVI Player 인스턴스를 정리(dispose) 합니다.
- DOM과 메모리 모두에서 COVI Player 인스턴스를 제거하는 유일한 방법입니다.

<br>

## 이벤트 트래킹 로그 전송 체크

### 1) 이벤트 트래킹(Event Tracking)
광고 캠페인의 성과를 측정하기 위해 사용되며, 상황에 따른 유저의 행동을 추적하는 트래킹 로그를 전송합니다.

<br>

### 2-1) COVI 플레이어 광고 영상 진행 시간에 따른 이벤트 트래킹 체크

<img width="770" alt="image" src="https://github.com/covigroup/covi-html5-player_with_React/assets/122589688/88476abc-5cc8-4818-b009-21db065816ad">

- ① 크롬 브라우저에서 개발자 도구(F12 / opt+cmd+i)를 엽니다.

- ② 네트워크 탭으로 이동합니다.

- ③ 필터창에 ‘covi’를 입력합니다.

- ④ Fetch/XHR 탭을 클릭합니다.

- ⑤ `imp`, `atp`, `sec2`, `vimp`, `qtr1`, `qtr2`, `qtr3`, `qtr4`, `sec15`, `sec30` 트래킹 로그가 정상적으로 전송됐는지(status:200) 확인합니다.

<br>

### 2-2) 클릭을 통해 광고주 랜딩 페이지로 이동했을 때 이벤트 트래킹 체크
- `click` 트래킹 로그가 정상적으로 전송되는지(status:200) 확인합니다.

![image](https://github.com/covigroup/covi-html5-player_with_React/assets/122589688/796f55ea-e4c2-4490-aac0-324d62b8f44f)

<br>

### 3) 이벤트 트래킹 로그별 전송 시점
- **`imp`** : 비디오 플레이어가 화면에 100% 노출 시 `imp` 로그를 전송합니다.
- **`start(atp or ctp)`** : 광고 재생 시작 시 전송되며 autoplay일 경우 `atp`, clicktopaly일 경우에는 `ctp` 로그를 전송합니다.
- **`sec2`, `vimp`** : 광고 영상이 2초 재생됐을 때 `sec2`, 3초 재생됐을 때 `vimp` 로그를 전송합니다.
- **`qtr1`, `qtr2`, `qtr3`, `qtr4`** : 광고 영상 길이(duration)를 기준으로 1/4, 2/4, 3/4, 4/4 재생 됐을 때 각 로그들을 전송합니다.
- **`sec15`, `sec30`** : 광고 영상이 15초 재생됐을 때 `sec15`, 30초 재생됐을 때 `sec30` 로그를 전송합니다. 
  + ex) 15초 길이의 광고 영상은 `sec15` 로그까지만 전송하고, 30초 길이의 광고 영상은 `sec15, sec30` 로그를 모두 전송합니다.
- **`click`** : 유저가 광고 영상 재생중에 플레이어 영역을 클릭 하거나 영상 종료 후 랜딩버튼(ex: 더 알아보기)을 클릭 했을 때 `click` 로그를 전송합니다.

<br>

## 가시성 측정 체크

### 1) 가시성(Viewability)
유저에게 게재된 광고가 보이는지 유무를 바탕으로 하는 지표로 COVI 플레이어가 가시적(viewable)인지를 측정합니다.

### 2) 가시성 측정 프로세스

<img width="1200" alt="image" src="https://github.com/covigroup/covi-html5-player_with_React/assets/122589688/4d7c948b-e550-4f02-8240-49b83f108098">

### 3) COVI 플레이어의 가시성 측정이 정상 작동하는지 체크하기

- [x] 화면에 동영상 플레이어가 100% 노출 될 때 재생 되는지 확인
- [x] 화면에 동영상 플레이어기 100% 노출 안될 때 일시 정지 되는지 확인

<br>

## 참조
|Topic|URL|
|---|:---:|
|covi-html5-player 개발 가이드|https://github.com/covigroup/covi-html5-player/wiki|
|React 함수형 컴포넌트 공식문서|https://react.dev/blog/2023/03/16/introducing-react-dev|
|useEffect Hooks|https://react.dev/reference/react/useEffect|
|usehooks 라이브러리 / useScript|https://usehooks.com/usescript|

