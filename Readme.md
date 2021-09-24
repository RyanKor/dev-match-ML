# Programmers Dev-Match 미술 작품 분류하기 프로젝트

## 프로젝트 작업 과정에 대한 내 코드 해설 포스트

- [RyanKor](https://equus3144.medium.com/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%8D%B0%EB%B8%8C%EB%A7%A4%EC%B9%AD-%EB%A8%B8%EC%8B%A0%EB%9F%AC%EB%8B%9D-%EA%B3%BC%EC%A0%9C-%ED%9B%84%EA%B8%B0-with-%ED%85%90%EC%84%9C%ED%94%8C%EB%A1%9C%EC%9A%B0-4b2b633b3e75)

## 1. 프로젝트 수행 기간

- 2021.09.17 ~ 2021.09.24

- 총 8일

## 2. 개요

### 1. Simple CNN 시도하기

- 1번째 시도 : Tensorflow in Practice (텐서플로우 자격증 취득을 위한 강의)에서 배운대로 단순하게 CNN을 적용했고, 첫 시도에서 train data를 valid와 분리하지 않고 사용하면서 손실율이 40.xxx 형태로 말도안되는 수치로 보이던 게 기억에 남는다. (valid data에 test data를 넣고 실행하는 본격 바보짓 진행)

![image](https://user-images.githubusercontent.com/40455392/133774186-5b37ad8d-8143-4ae9-977b-a03cc96c6703.png)

- 2번째 시도 : 케라스 창시자에게 배우는 딥러닝, Do It! 딥러닝 프로그래밍 책 2권을 참고하면서 이전에 과제에서 했던 코드를 다시 봤고, train/valid로 데이터를 분리하고, 검증 작업이 반드시 포함되어야한다는 것을 깨닫게 되면서 재 작업 시작.
    - 이에 따라 1번째 시도에서 해볼 때 나타나던 검증 데이터가 들쑥날쑥하던 것이 조금 줄었지만, 테스트 정확도는 결국 개선이 안되는 상황으로 마무리
    - (그러나 훈련이 반복될 수록 valid 데이터의 정확도가 떨어지는 상황 발생)

![image](https://user-images.githubusercontent.com/40455392/133923958-ccc61a98-e3ab-4f89-81b3-537efe093f99.png)

- 3번째 시도 : 하이퍼 파라미터를 이것저것 수정해보면서 사실 시도로만 치면 8번이 넘는데 최적화 함수의 학습률을 조절하고 (대체적으로 1e-4로 수렴), 배치 사이즈를 조금씩 조절해보면서 훈련을 시켜보니 검증 데이터가 훈련이 반복될 수록 정확도가 향상되는 것을 볼 수 있었다.

    - 이 시점에서 테스트 정확도가 잘해봐야 20%를 조금 넘어서 다른 사람들의 프로그래머스 과제 후기를 읽어봤고, (대략 프로젝트 수행 5일 시점) 에폭을 100번을 진행하는데도 내 학습 향상 속도가 현저히 낮아서 (에폭 100번 수행시 60% 조금 넘는 상황) 뭔가 잘못되었다고 판단.

    - 케라스에서 제공하는 이미지 전처리 모델들을 찾아보기 시작

![image](https://user-images.githubusercontent.com/40455392/133996171-fa65334c-7b49-4581-83df-e92d84c477ad.png)

### 2. Keras 이미지 전처리 모델 시도하기

![image](https://user-images.githubusercontent.com/40455392/134616305-4f14ce64-6666-45ef-a2c9-ce3f13fa96f4.png)

- 모델을 코세라 때처럼 하나씩 빌드하는 것은 너무 시간이 오래 걸릴 것 같았고, 3번째 시도 때 봤던 다른 사람들의 과제 정답은 파이토치로 제출되어 있는 글이 전부라서 (컴퓨터 비전은 파이토치가 최고라는 마지막 말과 함께...) 일단 배운건 텐서플로우와 케라스니까 이걸로 학습하자고 스스로 다독이며 문서 탐색 시작

- 기존에 사용하고 싶었던 모델로 점찍어둔 것은 Efficient Net이었으나 케라스 이미지 전처리 API 에선 아직 학습 성능이 표기 되어 있지 않았다.

- 그래서 CNN 처음 배울 때 언급되었던 ResNet50, ResNetV502를 먼저 적용해봤고, 당근 마켓 기술 블로그에서 2018년 인턴을 수행한 개발자의 Xception 사용 후기글이 있었던 것이 기억나서 각각 적용해봤다.

- 결과는 CNN만 사용할 때보다 월등하게 좋았다.

- 특별히 더 추가한 코드가 있던 것도 아니고 전이 학습만 추가했을 뿐인데 에폭 30번만으로 정확도 최소 95%가 나왔고, 손실율도 1% 내외였다.

- 사실 이미 만들어진 API를 사용하는 상황이라 코드에서 큰 차이는 없었지만, 이미지 사이즈를 조절해주는 정도가 전부였고, ResNet은 코세라로 배워서 어느 정도는 이해하고 있었지만, Xception은 아얘 모르는 모델이라 결과가 왜 ResNet 모델들을 쓸 때와 큰 차이가 없었는지 이해하지 못했다.

- 그럼에도 CNN을 사용할 때보다 정확도는 훨씬 높았고, 개선의 여지가 필요하지만, 레이블링까지 포함시켜서 최초로 내가 작업했다는 것이 매우 뿌듯했다.

- 아직까지 감이 잘 안오는 것은 왜 CNN만 사용할 때보다 어느 부분에서 개선이 되어서 훈련 데이터 정확도와 검증 데이터 정확도가 개선되었는지 (이건 논문을 일단 보자) 찾아봐야한다는 점.

![image](https://user-images.githubusercontent.com/40455392/134616828-7e42b28e-e13a-442d-bcd8-40cc8750913c.png)

- 그래도 초반엔 천둥벌거숭이 마냥 이것저것 막 적용하다가 조금씩 뭘 적용해야할지 보이는 게 기분이 좋다.

## 5. 최종 점수

- 제출 최고 점수 : 22점

- 점수에 대한 느낌 : Train/Valid에서 CNN을 사용할 때 마다 손실율을 획기적으로 감소시켰고, 정확도가 95%를 넘기고 손실율이 거의 1% 가깝게 보일 때 당연히 테스트 결과도 좋을 거라 생각했는데, 생각보다 제출한 점수는 높지 않았다.

- 아마 제출을 위해 몇 가지 세팅을 더 해보거나 테스트 데이터 훈련에서 1 ~ 2 가지를 빼먹은 것 같아서 다른 사람들의 풀이를 봤었는데 전부 파이 토치라서 제출 과정에서 어느 부분이 잘못되었는지 선뜻 이해하기가 어려웠다.

- 무엇보다 스스로 약속한 기간을 하루 넘어가면서까지 시도했는데도 점수가 높이 않아서 매우 당황했다.

- 일단 9월 말까지 캐글 프로젝트도 하나 해야하니, 캐글도 어느 정도 마무리 되면 다시 돌아와서 객관적인 시점에서 어느 부분이 잘못되었는지 확인해보자.

- 확실히 이미 전처리가 되어 있는 이미지를 활용하니, 정확도와 손실율이 획기적으로 낮아지는 것은 강의에서 제공하는 과제를 할 때와는 다른 관점에서 기분 좋은 일은 것 같다

- 적용한 전이 학습 모델들에 대한 이해가 부족하다. 각 모델들의 논문을 읽어보고 왜 테스트 정확도가 끝끝내 개선되지 못했는지, 그리고 내가 어떻게 개선할지 방향성을 잡아가는 시간이 필요할 것 같다.
