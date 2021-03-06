# 다층 신경망의 학습

* 다층 신경망은 선형 분리가 불가능한 문제도 모델링 가능하다.

## 역전파 알고리즘

* 은닉 노드들의 오차를 결정하는 방법.

* 은닉층 노드들의 오차를 결정한 다음 델타 규칙에 따라 오차들로 가중치를 조절한다.

* 신경망의 출력 오차를 출력층에서 시작해 입력층 바로 앞 은닉층까지 역순으로 이동한다.
* 다양한 가중치 갱신식의 변형이 개발된 이유는 신경망의 학습 안전성과 속도를 높여 학습을 잘 되게 하기 위해서이다.

```matlab
function [W1, W2] = BackpropXOR(W1, W2, X, D)
  alpha = 0.9;

  N = 4;  
  for k = 1:N
    x = X(k, :)';
    d = D(k);

    v1 = W1*x;
    y1 = Sigmoid(v1);    
    v  = W2*y1;
    y  = Sigmoid(v);

    e     = d - y;
    delta = y.*(1-y).*e;

    e1     = W2'*delta;
    delta1 = y1.*(1-y1).*e1;

    dW1 = alpha*delta1*x';
    W1  = W1 + dW1;

    dW2 = alpha*delta*y1';    
    W2  = W2 + dW2;
  end
end
``` 

## 비용함수

* 신경망의 지도학습에서 학습규칙은 비용함수로부터 유도된다.
* 어떤 비용함수를 선택하느냐에 따라 학습규칙과 신경망의 성능이 달라진다.
* 과적합 문제에 대처하기 위한 기법인 정착화 기법도 비용함수를 수정하는 방식으로 이루어진다.
* 비용함수를 사용하면 신경망의 출력 오차를 0으로 만들어도 연결 가중치의 값이 크면 비용함수는 여전히 큰 값을 가지게 된다. 따라서 비용함수의 값을 낮추려면 신경망의 출력 오차를 줄이는 동시에 가중치 크기도 되도록 작게 만들어야 한다.
