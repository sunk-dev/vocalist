# useNavigate

Link컴포넌트를 사용하지 않고 라우팅과 관려된 작업을 할때 사용하는 유용한 API 제공

import { useNavigate } from 'react-router-dom';

import { useDispatch, useSelector } from 'react-redux';

navigate('/library') -> //library 경로로 이동

# useEffect
리액트 컴포넌트가 랜더링 될때마다 특정작업을 수행하도록 설계 HOOK

  useEffect(() => {
    if (content === null) navigate('/library');
    if (!content) navigate('/library');
  }, []);
->두번째에 빈배열을 넣은 이유: useEffect에서 설정한 함수를 컴포넌트가 화면에 맨 처음 랜더링 될때만 실행해주고 업데이트 될때는 실행하지 않기 위해서

특정 값이 업데이트 될때만 실행하고 싶다면 두번째 빈배열에 검사하고 싶은 값을 넣어주면 됨

#redux 
컴포넌트 내부에서 내장함수 dispatch를 사용할수 있게 해줌
사용법: 
```js
import { useDispatch, useSelector } from 'react-redux';
const dispatch = useDispatch();
dispatch({type:'SAMPLE_ACTION'})
//ex) dispatch(setMessage(''));
```

- useDispatch 를 사용할때는 useCallback와 함께 사용하는 습관들이기

# useCallback

렌더링 성늘을 최적화 해야하는 상황에서 사용 -> 만들어놨던 함수 재사용가능

# useSelector
connect 함수를 사용하지 않고 리덕스의 상태를 조회가능함

const 결과=useSelector();

 const { content, remade, firstComplete, message } = useSelector(
    ({ data }) => ({
      content: data.content,
      remade: data.remade,
      firstComplete: data.firstComplete,
      message: data.message,
    }),
  );


  const makeNewContent = () => {
    if (content && !remade) { //content 가 있고 remade(다시 만드는게 아니라면)
      let tmp = content.split('\n').map((word) => {
        let i;
        if (word.includes(')')) i = word.indexOf(')'); //단어에 소괄호가 포함되어있으면, i는 소괄호의 인덱스번호
        else i = word.indexOf(' '); //그게 아니면 i는 공백의 인덱스번호
        return [word.slice(0, i), word.slice(i + 1)];//기준점이되는 i전후의 단어를 묶어서 반환
      });
      tmp = tmp.filter((w) => w[0] !== '');//[word.slice(0, i), word.slice(i + 1)] w[0],즉 반환된 배열의 첫번째가 빈문자열이 아닌것만 필터링해서 반황.

      dispatch(remadeContent(tmp));
    }
  };


