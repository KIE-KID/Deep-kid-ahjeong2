# Ch 05 - 순환 신경망(RNN)



### 1. rnnlm_gen.py



- Score 의 shape : (1, 1, 10000) → Ex)  [[[-0.00445436 -0.00227842 -0.0010397  ... -0.00125923 -0.00101773
      0.00159206]]]

-  x 의 shape : (1, 1) → Ex) [4601]

- word_ids  : [316, 9098, 7157, 812, 2012, 4840, 9968, 3256, 3481, 9036, 3712, 3449, 9194, 2952, 7815, 5157, 2421, 5419, 7869, 3873, 567, 2975, 7602, 5335, 996, 3106, 8574, 8478, 6506, 2956, 7106, 5674, 3621, 7979, 2226, 7431, 1066, 9531, 2116, 9067, 2229, 1803, 6676, 6576, 7003, 127, 3443, 946, 6273, 9030, 7128, 9311, 9150, 2277, 7212, 5168, 4414, 8244, 263, 4131, 6017, 3235, 6653, 8371, 7253, 2009, 1582, 5716, 5218, 1090, 8508, 7783, 2172, 6449, 9710, 4510, 2659, 6764, 3285, 1964, 7597, 9001, 1147, 774, 3788, 7101, 1064, 8632, 2655, 9558, 4331, 6653, 3133, 6015, 1327, 2077, 4289, 4809, 4904, 2375]

  → id 에 해당하는 word 로 바꿔주며 문장을 생성한다.

- 각 단어의 점수들을 softmax (p.reshape(100, 100))

  ```python
  각 단어 확률 정규화 :  [[1.00272984e-04 1.00376761e-04 1.00181780e-04 ... 9.96126982e-05
    1.00087229e-04 1.00267614e-04]
   [1.00199562e-04 1.00200094e-04 1.00209174e-04 ... 9.98907781e-05
    1.00042227e-04 9.97550669e-05]
   [1.00354620e-04 1.00105142e-04 1.00523204e-04 ... 9.98831747e-05
    1.00668069e-04 1.00727746e-04]
   ...
   [1.00389494e-04 9.99821787e-05 9.98417090e-05 ... 9.99552940e-05
    1.00009362e-04 9.99937838e-05]
   [1.00048244e-04 1.00146724e-04 1.00285157e-04 ... 9.99937256e-05
    9.98443647e-05 9.97578536e-05]
   [9.96107119e-05 1.00637844e-04 9.99856711e-05 ... 1.00456797e-04
    9.98362593e-05 1.00497011e-04]]
  ```

- sampled : 확률 분포 p 로부터 **다음 단어**를 샘플링

  ```python
  sampled = np.random.choice(len(p), size=1, p=p)     # 확률분포 p 로부터 다음 단어를 샘플링한다.
  sampled :  [1937]
  sampled :  [4897]
  sampled :  [4022]
  sampled :  [4469]
  sampled :  [7780]
  sampled :  [1945]
  sampled :  [5348]
  sampled :  [2594]
  sampled :  [4379]
  sampled :  [4530]
  sampled :  [6426]
  sampled :  [798]
  sampled :  [7684]
  sampled :  [148]
  sampled :  [7772]
  sampled :  [1971]
  sampled :  [396]
  sampled :  [4118]
  sampled :  [8253]
  sampled :  [8791]
  sampled :  [836]
  sampled :  [6660]
  sampled :  [2804]
  sampled :  [429]
  sampled :  [6645]
  sampled :  [9452]
  sampled :  [9984]
  sampled :  [1399]
  sampled :  [594]
  sampled :  [2694]
  sampled :  [6865]
  sampled :  [7909]
  sampled :  [2151]
  sampled :  [1815]
  sampled :  [4660]
  sampled :  [4220]
  sampled :  [7886]
  sampled :  [7729]
  sampled :  [870]
  sampled :  [3523]
  sampled :  [9543]
  sampled :  [2663]
  sampled :  [4639]
  sampled :  [1219]
  sampled :  [9307]
  sampled :  [5883]
  sampled :  [108]
  sampled :  [8343]
  sampled :  [310]
  sampled :  [6574]
  sampled :  [8020]
  sampled :  [9371]
  sampled :  [7496]
  sampled :  [5327]
  sampled :  [5935]
  sampled :  [2777]
  sampled :  [8060]
  sampled :  [6012]
  sampled :  [3237]
  sampled :  [1236]
  sampled :  [4130]
  sampled :  [6584]
  sampled :  [8048]
  sampled :  [1200]
  sampled :  [8653]
  sampled :  [2153]
  sampled :  [509]
  sampled :  [9779]
  sampled :  [5194]
  sampled :  [103]
  sampled :  [8156]
  sampled :  [9449]
  sampled :  [365]
  sampled :  [7387]
  sampled :  [3741]
  sampled :  [2173]
  sampled :  [6061]
  sampled :  [2090]
  sampled :  [1406]
  sampled :  [6128]
  sampled :  [8064]
  sampled :  [2783]
  sampled :  [5102]
  sampled :  [3300]
  sampled :  [4701]
  sampled :  [2933]
  sampled :  [1248]
  sampled :  [1492]
  sampled :  [7910]
  sampled :  [8256]
  sampled :  [1604]
  sampled :  [771]
  sampled :  [3467]
  sampled :  [9768]
  sampled :  [3327]
  sampled :  [1917]
  sampled :  [8677]
  sampled :  [9587]
  sampled :  [1490]
  ```

- np.random.choice(len(p), size = 1, p=p)

  - len(p) : 1차원 배열 또는 정수 👉 **배열 p 의 요소 중** 
  - size = 1 : 정수 또는 튜플(튜플인 경우 행렬로 리턴) 👉 **1개를 출력한다.**
  - p = p : 1차원 배열, 각 데이터가 선택될 확률 👉 **배열 p 에서 각 데이터의 확률 (확률의 합은 1이 되어야 함)**

