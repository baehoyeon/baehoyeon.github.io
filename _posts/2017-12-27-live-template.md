---
layout: post
title: "[ETC] LiveTemplate"
author: "Qoo"
---

# Live Template
자주 사용하거나 사용자 지정하는 코드 구조를 소스 코드 파일에 빠르고 효율적이며 정확하게 삽입할 수 있습니다.

## 변수선언
```
  $<variable_name>$
```
변수선언은 위와 같은 포멧으로 작성합니다.

### example
#### react-component
```javascript 1.6
import React, { PropTypes } from 'react';

const propTypes = {
};

class $COMPONENT$ extends React.Component {
    constructor(props) {
        super(props);
    }

    render() {
        return(
            <div>
              $COMPONENT$
            </div>
        );
    }
}

$COMPONENT$.propTypes = propTypes;
export default $COMPONENT$;
```
#### react-component-redux
```javascript 1.6
import React, {PropTypes} from 'react';
import {connect} from 'react-redux';

const stateToProps = state => ({});

const actionToProps = {};

@connect(stateToProps, actionToProps)
export default class $COMPONENT$ extends React.Component {
    static propTypes = {};
    
    render() {
        return(
            <div>
              $COMPONENT$
            </div>
        );
    }
}

```

#### react-component-stateless 
```javascript 1.6 
import React, {PropTypes} from 'react';

const propTypes = {};

const $COMPONENT$ = ({}) => (
    <div>
      $COMPONENT$
    </div>
);

$COMPONENT$.propTypes = propTypes;
export default $COMPONENT$;
```

### 적용
Intellij 의 Live Template 메뉴에 들어가서 
abbreviation 에 사용하고 싶은 약어를 작성한 후 example 처럼 template 을 추가한다.

그 후 코드에서 약어를 입력하면 위와같은 템플릿을 사용할 수 있다.

더 자세한 내용은 
[Jetbrains:LiveTemplate](https://www.jetbrains.com/help/idea/live-templates.html)
이 곳에서 확인하면 된다.