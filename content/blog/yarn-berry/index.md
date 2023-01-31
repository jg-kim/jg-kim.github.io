---
title: Yarn berry 사용하기
date: 2022-12-22 13:51:48
tags: ['node','npm','yarn']
---
<img src="./yarn.png">

# yarn?

npm(node package manager)의 단점을 보안하고자 페이스북에서 `Yet Another Resource Negotiator`인 yarn을 발표했다. yarn은 npm 기반으로 설계되었으나 설치 프로세스의 속도를 높이기 위해 작업을 병렬화하여 개선했다.
> 그외 안정성 항샹을 위한 lock file 도입을 하였으나 추후 npm에서도 추가됨

이후 2020년부터 1.x 버전을 유지보수 모드로 전환하며 **yarn classic**으로 이름을 변경했다.
그리고 2.x 버전부터는 **yarn berry**로 개발과 개선을 이뤄지고 있고 현재는 3.3 버전까지 진행되었다

## NPM과 yarn v1(classic)의 문제점
<img src="./node_modules.png">

의존성 관리를 위해 node_modules폴더를 이용하는 것이 특징인데, 이러한 방식은 의존성 검색은 비효율적으로 동작한다

라이브러리를 찾기 위해 순회하는 디렉터리 목록을 확인하려 할 때 node의 `require.resolve.paths`함수를 이용할 수 있다.

```
Welcome to Node.js v14.21.1.
Type ".help" for more information.
> require.resolve.paths('react')
[
  'C:\\Users\\jgkimm\\project\\ngm-execution-layer\\repl\\node_modules',
  'C:\\Users\\jgkimm\\project\\ngm-execution-layer\\node_modules',
  'C:\\Users\\jgkimm\\project\\node_modules',
  'C:\\Users\\jgkimm\\node_modules',
  'C:\\Users\\node_modules',
  'C:\\node_modules',
  'C:\\Users\\jgkimm\\.node_modules',
  'C:\\Users\\jgkimm\\.node_libraries',
  'C:\\Program Files\\nodejs\\lib\\node',
  'C:\\Users\\jgkimm\\.node_modules',
  'C:\\Users\\jgkimm\\.node_libraries',
  'C:\\Program Files\\nodejs\\lib\\node'
]
```
목록에서 확인한 것처럼 NPM은 패키지를 찾기 위해 상위 node_modules를 계속해서 탐색해기 때문에 많은 비용의 I/O작업을 반복적으로 진행하게 된다

## Plug n play
yarn berry의 pnp는 node_modules를 해결하기 위한 전략이다. node_modules를 생성하는 대신 모든 패키지는 `.yarn/cache`폴더 내부에 zip 파일로 저장된다. 그러므로 디스크 공간을 적게 차지하고 파일 수가 많이 생성되지 않기 때문에 git으로 의존성 파일을 관리할 수 있다.
의존성 파일을 git으로 관리하기 때문에 모든 환경에서 동일한 버전으로 관리가 쉽게 가능하기 때문에 zero-intall이 가능해진다

## install (yarn berry)
yarn classic과 다르게 현재는 corepack으로 설치를 진행한다

1. corepack 설치
    - Node >= 16.10의 경우 corepack이 내장되어 있기 때문에 enable만 하면 corepack 사용이 가능함
    ```
    corepack enable
    ```
    - Node < 16.10의 경우 corepack 설치가 필요
    ```
    npm i -g corepack
    ```
2. yarn berry 설치
    - Node.js ^16.17 or >=18.6
    ```
    corepack prepare yarn@stable --activate
    ```
    - Node.js <16.17 or <18.6
    ```
    corepack prepare yarn@<version> --activate
    ```

>yarn v1(classic)에서 업데이트의 경우 `yarn set version stable` 실행으로 가능하다

## vscode 설정
yarn berry의 경우 의존성 파일을 zip 파일로 관리하기 때문에 go-to-definition 기능을 사용하기 위해서는 추가 extension이 필요하다.
1. [ZipFs](https://marketplace.visualstudio.com/items?itemName=arcanis.vscode-zipfs) 설치

그리고 vscode의 sdks를 설치한다.

2. `yarn dlx @yarnpkg/sdks vscode`

설치가 완료되면 vscode 워크스페이스의 typescript의 버전을 설정을 `.yarn/sdks/typescript/lib`로 설정한다(자동으로 우측 하단에 allow관련 컨펌 토스트 알림이 발생).

## 마치며
CI에서 docker에 환경을 설정하고 의존성 파일을 내려받는 과정에 많은 시간이 걸렸는데, zero-install로 인한 시간 단축만으로 만족도가 높다. 물론 설정하는 과정과 기존 node_modules의 경로를 직접 참고(tsconfig의 paths)하는 부분이 있었는데 yarn berry를 적용하고 어떤 방식으로 수정을 진행해야 할지는 차차 찾아봐야 할 일이다

## 참조문서
https://toss.tech/article/node-modules-and-yarn-berry

https://yceffort.kr/2022/05/npm-vs-yarn-vs-pnpm

https://helloinyong.tistory.com/341