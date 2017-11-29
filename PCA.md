# 차원 축소 기법(Dimensionality Reduction Methods)



## PCA(Principal Components Analysis)

* PCA는 많은 변수들이 갖고 있는 정보를 더 적은 숫자의 principal components로 요약하는 기술이다.
* PCA는 비지도학습이다. 
* 쓰이는 분야
  * 변수들 간의 관계를 이해하기 위해서
  * 지도 학습을 돕기 위해서
  * 시각화를 위해서



### Principal Components란?

* PCA는 변수들이 갖고 있는 정보를 요약하려고 한다.

* 정보는 변동성(variation)이다.

  * 어떤 변수에 대해 데이터들이 모두 똑같은 값을 갖고 있다면 그 변수는 쓸모없다.
  * 즉 변동성이 없다면 아무 정보도 갖고 있지 않은 변수이다.
  * 변동성이 클수록 흥미로운(interesting) 변수이다.
  * 데이터들이 평균에서 많이 떨어져있는 변수일 수록 많은 정보를 갖고 있는 변수이다.
  * 참고
    * R^2 지표는 데이터가 가진 전체 분산 중에 모델이 설명하는 분산의 비율이었다는 걸 기억해보자.
    * 분산은 곧 정보이다.

* 이미 있는 변수들로 데이터의 변동성이 최대가 되는 새로운 변수를 만들 수 있다.

  * 데이터 셋의 변동성을 최대한 유지하는 저차원 표현(low-dimensional representation)을 만들자.
  * 여기서 저차원 표현이 principal components이다.

* 첫번째 principal component는 기존 변수들의 정규화된 선형결합(normalized linear combination)이다. 이 변수는 가장 큰 분산을 갖고 있어야 한다.

* $$
  z_1 = \phi_{11}x_1 + \phi_{21}x_2 + ... + \phi_{p1}x_p
  $$

* 정규화되었다는 건 다음과 같은 말이다.

* $$
  \sum^p_{j=1}\phi^2_{j1} = 1
  $$

* 정규화 하는 이유?

  * $phi$가 마음대로 커지면 분산도 마음대로 커질 수 있으니까

* 이 때 $\phi_{j1}$ 등을 첫번째 principal components의 loadings라고 부른다.

* principal component loading vector

* $$
  \phi_1 = (\phi_{11},  \phi_{21}, ..., \phi_{p1})^T
  $$

* 어떻게 구할까?

  * 일단 우리는 분산에만 관심 있으니까, 모든 변수의 평균은 0이라고 가정한다. (데이터에서 변수마다 평균을 빼는 표준화를 했다고 가정한다.)
  * 기존 변수들의 선형 결합으로 가장 큰 분산을 가진 변수를 만들자.

![pca](https://files.slack.com/files-pri/T25783BPY-F68K7NA9L/screenshot.png?pub_secret=e2ec95153c)

* z의 sample variance이다.
* z11, z21, …, z_n1을 첫번째 principal components의 score라고 부른다.
* Eigen decomposition으로 풀 수 있다. (생략)
* 첫번째 principal component를 구하고 나면, 그 다음 두번째를 구할 수 있다.
* 두번째 역시 기존 변수들의 조합으로 변동성이 최대가 되어야 한다. 단, 첫번째 principal component와 uncorrelated되어야 한다.
* uncorrelated된다는 것은 $\phi_2$가 $\phi_1$에 수직으로 만나는(orthogonal) 것임이 알려져 있다.



### 기하학적 해석

* loading vector $\phi_1$는 변수 공간(feature space)에서 방향을 결정한다.
* n개의 데이터 포인트를 이 방향에 투영(projection)시키면, 투영된 값들은 principal component scores z_11, …, z_n1이다.

![pca](https://files.slack.com/files-pri/T25783BPY-F675A8R7S/screenshot.png?pub_secret=459279c425)



### biplot

* 다음 그래프는 PCA를 미국 주의 범죄 데이터셋에 적용한 것을 나타낸 것이다.
* 변수는 네가지이다. UrbanPop, Rape, Assult, Murder
* 데이터는 미국의 각 주마다 50개이다.
* principal component score vector의 길이는 50이다.
* 첫 두 principal component를 나타낸 것이 아래 그래프이다.
* principal component scores와 loading vectors를 한 플롯에 그렸다. 이를 biplot이라고 한다.

![biplot](https://files.slack.com/files-pri/T25783BPY-F68KK94LW/screenshot.png?pub_secret=47ed7e2807)

* 해석
  * 첫번째 loading vector는 Assult, Murder, Rape에 대해 비슷하다는 것을 알 수 있다. UrbanPop에 대해서는 작다.
    * 이 component는 전반적인 범죄의 비율을 나타낸다고 볼 수 있다.
  * 두번째 loading vector는 UrbanPop에 weight를 많이 갖고 있다.
    * 도시화 정도를 나타낸다고 해석할 수 있다.
  * Murder, Assult, Rape는 비슷하고 모여 있다. 
    * 범죄 관련 변수들은 서로 상관관계가 높다. 즉, 살인이 많이 일어나는 주에서 성폭력도 많이 일어난다.
  * UrbanPop은 멀리 떨어져 있다.
    * 도시화 정도와 범죄율은 상관관계가 높지 않다.
  * 50개 주의 분포를 보고 어느 주가 범죄율이 높고 도시화율이 높은지 개략적으로 판단할 수 있다.



### Principal Components의 또다른 해석

* 지금까지 우리는 principal component loading vector를 데이터의 변동성이 최대가 되는 변수 공간(feature space)에서의 방향이라고 해석해왔다.
* 또다른 해석은, principal components는 데이터에 가장 가까운 저차원의 공간(low-dimensional linear surfaces)라고 해석하는 것이다. (즉 분산이 아니라 거리를 본다.)
  * 첫번째 principal component loading vector는 p차원의 공간에서 데이터에 가장 가까운 직선(line)이다.
  * '가깝다'라는 것은 유클리디안 거리의 제곱의 평균으로 측정된다.
  * 즉, 우리는 데이터에 가장 가까워서 데이터를 잘 요약할 수 있는 차원을 찾는 것이다.


* 첫번째 principle components에만 해당되는 것은 아니다.
  * 첫번째, 두번째 principal components는 데이터에 가장 가까운 평면이다.

![closest](https://files.slack.com/files-pri/T25783BPY-F67R91QKC/screenshot.png?pub_secret=7edca54cc7)

* 처음 M개의 principal component score vectors와 처음 M개의 principal component loading vectors는 데이터의 가장 좋은 M차원의 요약(근사, approximation)이다.

* $$
  x_{ij} \approx \sum^M_{m=1}z_{im}\phi_{jm}
  $$

* 만약 M이 변수(feature)의 개수와 똑같다면, 좌변과 우변은 정확하게 일치한다.



### PCA에서 고려할 것들



#### Variable scaling

* 변수들은 평균이 0이 되어야 한다. (위에서 보았듯이)
* PCA 결과는 변수들의 스케일에 따라서도 달라진다.
* 스케일이 큰 변수가 변동성이 큰 것으로 오해받기 때문이다.
* 어떤 경우에는 스케일링을 하지 않을 수도 있다. 스케일이 큰 변수가 변동성이 큰 게 맞는 경우.



![scale](https://files.slack.com/files-pri/T25783BPY-F67U5F9K5/screenshot.png?pub_secret=b1511d7baf)



### Principal Components는 유일하다

* 각각의 principal component loading vector는 유일하다. 
* +/- 부호 변화는 있을 수 있다.
  * 방향은 반대여도 똑같으니까
* score 역시 부호 차이는 있을 수 있다.
  * z의 분산은 -z의 분산과 똑같으니까



###  설명되는 분산의 비율

* PCA가 얼마나 데이터의 정보를 잘 요약할까? 얼마나 많은 정보가 손실될까?

* 즉, 얼마나 많은 분산이 처음 몇개의 principal components에 포함될까?

* proportion of variance explained (PVE)로 측정할 수 있다.

* 데이터의 총분산(total variance)은

* $$
  \sum^p_{j=1}Var(x_j) = \sum^p_{j=1}\frac{1}{n}\sum^n_{i=1}x^2_{ij}
  $$

* m개의 principal component로 설명되는 분산은

* $$
  \frac{1}{n}\sum^n_{i=1}z^2_{im} = \frac{1}{n}\sum^n_{i=1}(\sum^p_{j=1}\phi_{jm}x_{ij})^2
  $$

* 그리고 PVE는 m개의 principal component로 설명되는 분산을 총 분산으로 나눈 것이다.

* 이를 그래프로 나타낸 것을 scree plot이라 부른다.

* ![pve](https://files.slack.com/files-pri/T25783BPY-F6816EUQ2/screenshot.png?pub_secret=850240434b)



### 몇 개의 principal component를 써야 할까?

* scree plot을 보고 principal component가 설명하는 분산이 뚝 떨어질 때까지
  * elbow method
  * 위 그림에서 두개의 principal component가 상당히 많은 부분을 설명한다.
  * 절대적인 방법은 아니다.
* 객관적인 방법은 별로 없다.
  * 어쨌든 이것도 비지도학습(unsupervised learning)
  * PCA는 data exploration에서 많이 쓰인다.
* 만약 PCA 결과를 supervised learning에 활용한다면
  * principal components의 개수를 hyperparameter로 보고 조정할 수 있다.