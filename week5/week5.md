# 스터디 5주차

- [Lecture 9](#lecture-9)
- [Lecture 10](#lecture-10)

## Lecture 9

#### ✅ `소현` `p30` 파라미터 즉 가중치의 개수를 구할 때 (필터의 너비)_(필터의 높이)_(입력데이터의 차원)\*(출력데이터의 차원) 이렇게 되는데 입력 데이터의 차원(ex. RGB)에 해당되는 필터로 둔다! 이 부분은 이해가 가는데 출력데이터의 차원은 왜 고려하는지 직관적인 이해가 안갑니다. 그리고 CNN에서의 가중치를 필터의 값으로 생각했는데 맞나요? 그렇다면 필터의너비\*필터의높이\*필터의 개수가 가중치 즉 파라미터의 개수가 된다고 생각하는데 어떻게 생각하시나요?

#### ✅ `혜지` `p30` 파라미터 세는 거 이해 안감

![0.png](img/0.png)

첫번째 conv layer를 보자. conv-64는 3\*3필터 64개를 가진다. 그럼 저 파라미터를 어떻게 해석하느냐? 일단 가중치는 필터의 픽셀 개수이다! 이걸 꼭꼭 기억하자. 그럼 3\*3 필터니까 한 필터에 가중치 개수는 9개이다. 근데 필터는 입력 데이터의 각 차원에 연산을 진행하므로 한 필터는 3\*3\*3(입력 데이터의 가중치)개의 가중치를 갖는다. 근데 우리가 feature map을 몇개 뽑아낼 건지를 필터의 개수로 정해준다. 여기서 필터의 개수는 64개로, 총 파라미터의 개수는 3\*3\*3\*64개 이다.

#### ✅ `세연` `p30` [Stride가 1인 필터 세 개를 쌓게 되면 실질적인 Receptive Field가 어떻게 될까요?] pixel 15인 이유?

receptive field란 이미지 데이터에 해당 필터가 얼마만큼의 영역을 덮는가에 대한 것이다. 일단 저 15px이란 말은 틀렸다. 왜냐하면 stride 1이라 필터가 겹쳐지는 것을 고려하지 않았기 때문이다.

그럼 다시 돌아가서 3\*3 필터를 가진 conv 레이어 3개를 쌓았을 경우를 생각해보자. 우선 첫 번째 레이어에서 3\*3필터는 이미지의 3\*3만큼을 덮을 수 있다. 그러므로 receptive field는 9px이다. 두 번째 레이어는 첫 번째 레이어의 출력의 3\*3을 덮는다. 이를 원 이미지 데이터에 대해서 생각해보면 두 번째 레이어에 들어온 입력은 이미지의 3\*3을 덮는다. 하지만 stride 1이므로 필터는 겹쳐지기 때문에 9\*9를 보는 것이 아닌 5\*5(모든 사이드로 1px씩 더)만큼을 덮게 된다. 세 번째 경우도 두 번째 레이어와 마찬가지로 한 픽셀씩 모든 사이드로 추가되어 총 7\*7만큼을 덮게 된다. 이는 7\*7 conv layer의 receptive field가 같다.

#### ✅ `재용` `p52` input feature map 선형결합?

계산 자체가 선형 결합이라서 선형 결합이라는 말이 쓰인 것 같다. 그니까 100\*100\*5의 입력데이터가 있다고 해보자. conv1\*1의 경우 그냥 해당 필터가 덮는 부분의 모든 채널의 값들을 더하기만 한다. 그렇게 되니 Input feature map 즉 각 채널 끼리의 데이터의 선형 결합이란 말이 나온 듯 하다.

#### ✅ `소현` `p55` conv 1\*1이 필요한 이유. 강의의 설명이 잘 이해가 안되서 이해가 잘가는 이유를 같이 찾아보아요.

![1.png](img/1.png)

conv 1\*1은 결국 모든 채널의 값을 더하므로 저 56\*56\*64의 입력데이터를 conv 1\*1 필터 1개를 씌우면 56\*56\*1이 된다. 근데 32개의 필터가 있으므로 결과적으로 출력이 56\*56\*32가 된다. 일단 conv 1\*1를 쓰는 이유는 입력 데이터가 뭉게지지 않는다. 즉, 높이와 너비가 그대로 유지된다. 하지만 차원을 높은 차원에서 낮은 차원으로 바꿀 수 있으므로 차원을 줄이고 싶을 때 유용하다.

#### ✅ `세연` `p72` 전체 그대로 학습하는 것과 f(x)만 학습하는 것과 차이가 나는 이유를 잘 모르겠습니다.

결과론적으로 f(x)로 학습하는 것이 좋다. 이러이러한 이유에서 이 방법이 좋다라기 보다는 이렇게 해봤더니 이런 결과가 나와서 좋다 이런 식이다.

#### ✅ `재용` `p76` global average pooling

각 차원의 값들을 평균을 만들어서 벡터로 만드는 것. 예를 들어 100\*100\*5의 데이터가 있다면 1\*1\*5로 각 차원을 평균내서 만드는 것이다.

![2.png](img/2.png)

#### ✅ `재용` `p92` micronetwork

기존 네트워크랑 다르게 conv 연산만 하는게 아니라 conv 연산을 하고 fc 2개를 그 사이에 넣어서 하는 것 같은데.. 자세한 건 논문을 따로 찾아봐야할 듯 하다.

![3.png](img/3.png)

## Lecture 10

#### ✅ `혜지` `p26` 행렬 W의 기울기를 각 step에서 전부 계산한다는 게 입력 x를 기울기 미분한다는 의미? 같은 가중치를 쓰는 이유.

rnn은 sequence 데이터가 들어가기 때문에 데이터 하나가 하나가 끝이 아니라 일련의 timestamp로 되어 있는 데이터이다. 예를 들어 "hello"란 데이터가 있으면 h, e, l, l, o이렇게 rnn으로 들어가게 된다. 이 때 W는 h다음에 뭐가 올지, e 다음에 뭐가 올지, l 다음에 뭐가 올지에 대한 가중치값이다. 즉 timestamp를 가진 데이터가 다 들어와야 W가 학습이 되는 것. 그래서 h들어 올때 가중치 W, e 들어올 때 같은 가중치 W가 쓰이는 것이다.

#### ✅ `혜지` `p38` 왜 e가 뽑혔지?

score값이 높은 것만 뽑히는 것이 아니라 softmax로 확률을 계산하여 샘플린해서 결정한다. h가 0%, e 18%, l 2%, o가 80%라고 할 때 18%의 확률로 e가 뽑힌 것이다.

#### ✅ `재용` `30:00` markov

markov assumption이라는 것은 과거와 현재가 독립적이라는 것. 즉 과거의 일이 현재의 일에 영향을 끼치지 않는다는 것이다. RNN에서는 해당 가정은 성립하지 않는다.
