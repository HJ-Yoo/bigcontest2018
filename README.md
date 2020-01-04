# Your Financial Status
빅콘테스트 2018 이노베이션 금융분야 데이터 분석과정을 소개하는 repository입니다. 분석 과정을 담은 ppt, Rmd와 분석을 위해 생성한 R 함수들을 각각 업로드했습니다. 경연에 참가한 1,196팀 사이에서 <a href=https://kbig.kr/portal/kbig/keybiz/contest/status.page>최우수상(신한은행상)을 수상</a>했습니다.

## Overview
문제가 출제될 2018년 당시 신한은행은 자사의 모바일뱅킹 어플리케이션 SOL에 '보통사람 금융생활 보고서'라는 서비스를 개발하여 탑재하고자 했습니다. 당시 주어진 과제를 보다 구체적으로 설명하기 위해 신한은행이 당시에 제공한 데이터에 대한 설명을 먼저 하겠습니다. 과제 설명을 위해 사용할 개념어들은 굵게 표시했습니다.

### Data
신한은행은 17,076명에 대한 32가지 <b>고객정보</b>를 포함하고 있는 데이터를 제공했습니다. 그 32가지 고객정보는 아래와 같이 분류할 수 있습니다.

<ol>
  <li><b>고객기본정보</b>(괄호 안은 경우의 수)
    <ul>
      <li>필수입력 정보 5가지 : 성별(2), 나이 구분(5), 소득 구분(7), 주소 구분(5), 직업 구분(9)</li>
      <li>선택입력 정보 3가지 : 결혼 여부(3), 맞벌이 여부(3), 자녀 수(5) <br>
        - 선택정보이므로 입력을 하지 않는 옵션도 존재하고, 따라서 경우의 수로 포함</li>
    </ul>
  </li>
  <li><b>금융거래정보</b>
    <ul>
      <li>금융자산, 월 소비액, 월 저축액 등 24가지</li>
    </ul>
  </li>
</ol>

고객기본정보의 조합이 가질 수 있는 경우의 수는 2 * 5 * 7 * 5 * 9 * 3 * 3 * 5 = 141,750 입니다. 하나의 조합을 <b>고객유형</b>이라고 할 때, 고객유형의 가능한 경우의 수는 141,750임을 확인할 수 있습니다. 참고로 주어진 17,076명의 표본 데이터에는 중복을 제외하고 총 5,084개 <b>고객유형</b>들의 정보만 포함되어 있었습니다.

### Objective
'보통사람 금융생활 보고서'는 고객과 아래와 같은 정보를 교환하는 서비스입니다.
<ol>
  <li><b>고객기본정보가 같은 사람들의 평균 정보 제공</b>
  <ul>
    <li>고객 : 서비스 창에 최소 5개, 최대 8개의 고객기본정보를 입력</li>
    <li>서비스 : 그 고객과 고객기본정보 조합이 같은 사람들의 평균 금융자산, 평균 월 소비액, 평균 월 저축액 제공</li>
  </ul>
  </li>
  <li><b>고객기본정보가 비슷한 사람들 사이에서 백분위 점수 제공</b>
    <ul>
      <li>고객 : 입력한 고객기본정보에 더해 서비스 창에 자신의 금융자산, 월 소비액, 월 저축액을 입력 </li>
      <li>서비스 : 그 고객과 비슷한 사람들과 비교했을 때 그 고객의 금융자산, 월 소비액, 월 저축액의 백분위 점수 제공</li>
    </ul>
  </li>
</ol>
 
위와 같은 서비스를 제공하기 위해서 다음의 중요 정보들이 부족했고, 이를 추정해 내는 것이 과제로 주어졌습니다,
<ol>
  <li>임의의 고객유형에 대한 금융거래정보
    <br>5,084개 고객유형에 해당하는 고객들에게는 수집된 표본의 금융거래정보를 바탕으로 고객기본정보가 같은 사람들의 평균 정보를 제공할 수 있습니다. 문제는 남은 약 13만 6천개 고객유형에 해당하는 고객들의 금융거래정보가 부재하다는 것이고, 임의의 고객유형에 해당하는 고객들에게도 서비스를 제공하기 위해서는 그 모든 부재한 금융거래정보를 추정해야 했습니다. 따라서 다음과 같은 과제가 우선 주어집니다:
    <br><b>(1) 141,750개 고객유형의 고객정보 완성(즉, 24개 금융거래정보들의 평균 추정)</b></li>
  <li>'고객기본정보가 비슷한 사람들'의 정의
    <br>한 고객이 고객정보를 입력했을 때 '그 고객과 비슷한 사람들'이 사전에 정의되어 있어야 그 비슷한 사람들과의 비교를 통해 백분위 점수를 제공할 수 있을 것입니다. 신한은행은 고객정보를 활용한 군집분석을 통해 얻은 군집들 중 같은 군집에 속하는 고객들을 서로 비슷한 사람들이라고 정의했습니다. 그렇기에 모든 고객유형들의 군집정보가 필요하게 되고, 아래와 같은 과제가 주어집니다:
    <br><b>(2) 141,750개 고객유형 군집화
    <br>(3) 획득한 고객유형 군집들 각각의 금융자산, 월 소비액, 월 저축액의 백분위 도출</b> </li>
  <li>불필요한 고객기본정보 탐색
    <br>서비스를 이용하고자 하는 모든 고객은 일단 기본적으로 고객기본정보를 입력해야 합니다. 최대 8개를 입력하는 과정에서 고객이 입력에 대한 피로감을 느낀다면 서비스를 이용하지 않을 위험이 있습니다, 그렇기에 입력하지 않아도 서비스 제공에 타격이 없는 기본정보를 찾아내는 것이 선택 과제로 주어졌습니다:
    <br><b>(4) 잉여 고객기본정보 탐색 및 근거 제시</b>
  </li>
</ol>

## Contents
이러한 과제를 아래와 같은 순서로 해결했습니다. Bayesian Network를 통한 변수 간 조건부독립 관계 탐색을 핵심 테마로 하여 분석을 진행했습니다.
<ol>
  <li>주어진 표본의 고객유형 군집화(PAM Clustering)</li>
  <li>모든 고객유형에 군집 할당(Bayesian inference)</li>
  <li>Gibbs Sampling을 통한 금융거래정보 추정
    <ol>
      <li>군집별 고객정보의 결합확률분포 내의 조건부독립 관계 탐색(Bayesian Network)</li>
      <li>Kernel Density Estimation을 통해 얻은 조건부 분포를 사용한 샘플 생성(Acceptance-Rejection sampling)</li>
      <li>생성한 샘플들로부터 24개 금융거래정보들의 평균 추정 후 군집별 금융자산, 월 소비액, 월 저축액 백분위 획득</li>
    </ol></li>
  <li>잉여 고객기본정보 탐색(Markov Blanket)</li>
</ol>
