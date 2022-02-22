---
title: Pages 사이트 비교해보기
author: Yongjun042
date: 2022-01-02 22:55:00 +0900
categories: [Blog, Develope]
tags: [React, CRA, webpack]

published: false
---

CRA(Create React App)으로 리액트 앱을 만들고 웹팩 5을 적용하게 되면 웹팩 5부터 polyfill 을 기본으로 지원하지 않기 때문에 오류가 생긴다. polyfill은 브라우저 호환성을 위한 도구지만 불필요한 경우가 많아 제외되었다.

```
ERROR in ./node_modules/readable-stream/lib/_stream_readable.js 46:13-37

Module not found: Error: Can't resolve 'buffer' in 'C:\ *\node_modules\readable-stream\lib'

BREAKING CHANGE: webpack < 5 used to include polyfills for node.js core modules by default.
This is no longer the case. Verify if you need this module and configure a polyfill for it.

If you want to include a polyfill, you need to:
	- add a fallback 'resolve.fallback: { "buffer": require.resolve("buffer/") }'
	- install 'buffer'
If you don't want to include a polyfill, you can use an empty module like this:
	resolve.fallback: { "buffer": false }

```

해당 오류를 해결하려면 웹팩 설정을 수정해야 되는데 `npm eject`를 사용할 경우 CRA의 장점인 편리함을 잃게 되기 때문에 `customize-cra`와 `react-app-rewired`를 이용해 수정을 해준다. 웹팩뿐만 아니라 바벨도 수정 가능하다.

``` console
yarn add react-app-rewired customize-cra --dev
npm install --save-dev react-app-rewired customize-cra
```


패키지 설치 후 cofig-overides.js파일을 최상단에 생성 후 오류에 맞춰 작성해준다.


``` javascript
// config-overrides.js
const { override, loaders } = require('customize-cra');

module.exports = function override (config, env) {
    console.log('override')
    let loaders = config.resolve
    loaders.fallback = {
        "buffer": false,
        "crypto": require.resolve("crypto-browserify"),
        //... 추가 설정
    }
    
    return config
}
```

해당 기능이 필요없는 경우 `false`로 사용하지 않고, 필요한 경우 해당 패키지를 설치한 후 `require.resolve()`로 적용시켜주면 된다.

``` json
// pacakage.json
{
  "scripts": {
    "start": "react-app-rewired start",
    "build": "react-app-rewired build",
    "test": "react-app-rewired test",
    ...
  },
  ...
}
```

그리고 다음과 같이 `react-script ...`부분을 `react-app-rewired`로 바꿔 적용시켜주면 된다.
