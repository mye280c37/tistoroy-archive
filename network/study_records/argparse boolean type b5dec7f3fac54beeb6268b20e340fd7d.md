# python argparse boolean type 받기

## problem

```python
import argparse

parser = argparse.ArgumentParser()
parser.add_argument("--feature", default=False, type=bool)
```

다른 int나 str 타입을 정의하듯이 `type=bool` 로 하고 `add_argument()` 함수를 실행하면 어떤 값을 넣어도 해당 값이 문자열로 인식돼서 해당 argument의 값이 항상 `True`로 나온다.

## solution

`type`에 함수를 넣어서 arg로 들어온 문자열을 boolean으로 바꿔주면 된다.

```python
parser.add_argument("--starvation_field", default=False, type=lambda x:(True if x=='True' else(False if x=='False' else argparse.ArgumentTypeError('Boolean value expected.'))), help="starvation field on")
```

따로 함수를 정의해서 넣어줘도 되지만 나의 경우 해당 argument만 boolean type으로 사용할 예정이라 lambda를 이용해 해결하였다.

## reference

- [Parsing boolean values with argparse](https://stackoverflow.com/questions/15008758/parsing-boolean-values-with-argparse)
- [[Python] argparse(Argument Parser) 에서 boolean 값 받기](https://eehoeskrap.tistory.com/521)