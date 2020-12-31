---
title: Face Detection Technology
excerpt: 얼굴 인식 기술을 활용한 실습
search: true
categories: 
  - AI
tags: 
  - AI
last_modified_at: 2020-12-24
---

# Face Detection (얼굴인식)



```
pip install face_recognition
```



* pip(패키지 관리자)는 파이썬으로 작성된 패키지 소프트웨어를 설치 · 관리하는 패키지 관리 시스템이다

```python
import cv2
import face_recognition as fr
from matplotlib import pyplot as plt
```

- cv2 : opencv패키지

- matplotlib : 파이썬에서 시각화로 쓰이는 대표적인 package 중 하나

  ​					 **데이터 시각화 도구** (그래프와 같은 형태로 시각화)

  ​					 다양한 데이터를 많은 방법으로 도식화 할 수 있도록 하는 파이썬 라이브러리

  ​					 나중에 numpy나 pandas를 사용할 때 데이터 시각화에 유용

  ​					 sub package - pyplot : 시각화

  ​											- pylab   : 시각화 + numpy (수학 관련 pkg)

  

- pyplot : matplotlib 라이브러리의 pyplot 모듈은 그래프를 그리는 데 사용되는 다양한 함수 포함하고 있다



```python
image_path = "/gdrive/My Drive/colab/bts.jpg"

image = fr.load_image_file(image_path)
face_locations = fr.face_locations(image)

```

- image에는 이미 face recognition 이 적용된 이미지가 들어있음

  >좀 더 이해하기 쉽게 표현한 이미지
  >
  >![image-20201223201137772](https://user-images.githubusercontent.com/47768081/102999214-fb029600-456b-11eb-8472-6b8da2d46911.png)

  

- face_locations에는 얼굴 이미지가 인식된 부분의 좌표값이 담김 (top, right, bottom, left)

```python
for(top, right, bottom, left) in face_locations:
  cv2.rectangle(image, (left, top), (right, bottom), (0,255,0), 3)
```

- 얼굴을 인식할 부분의 사각형 그리는 코드
- (0, 255, 0) 은 RGB 색상
- 여기서 3은 사각형 두께
- 해당 코드 부분 이미지 :

![image-20201223193850631](https://user-images.githubusercontent.com/47768081/102999313-21c0cc80-456c-11eb-8ee8-e2f2b8a2d24e.png)



```python
# 이미지 버퍼 출력
plt.rcParams["figure.figsize"] = (16,16)
plt.imshow(image)
plt.show()
```

- (16, 16) 은 사진의 크기! 얼굴인식하는 rectangle 크기 X
- 출력 화면 :

![image-20201223194147297](https://user-images.githubusercontent.com/47768081/102999358-32714280-456c-11eb-8d83-864c80a01ad4.png)



### [실습] person 4명과 unknown 이미지를 활용해서 동일인 찾기


```python
plt.rcParams["figure.figsize"] = (1,1)

known_person_list = []
known_person_list.append(fr.load_image_file("/gdrive/My Drive/colab/person1.jpg"))
known_person_list.append(fr.load_image_file("/gdrive/My Drive/colab/person2.jpg"))
known_person_list.append(fr.load_image_file("/gdrive/My Drive/colab/person3.jpg"))
known_person_list.append(fr.load_image_file("/gdrive/My Drive/colab/person4.jpg"))
```

- 이미지 파일을 로드하여 known_person_list 리스트 생성

  

```python
known_face_list = []
for person in known_person_list:

  # 얼굴 좌표를 알아내서 잘라낸다
  top, right, bottom, left = fr.face_locations(person)[0]
  face_image = person[top:bottom, left:right]

  known_face_list.append(face_image)
```

-  known_face_list : 얼굴을 인식을 하여 감지된 부분을 잘라낸 다음 known_face_list에 저장
-  known_face_list에 잘라낸 face_image를 저장



```python
for face in known_face_list:
  plt.imshow(face)
  plt.show()
```

- known_face_list에 저장된 얼굴들 출력

- 출력 화면 : 

  ![image-20201223200551082](https://user-images.githubusercontent.com/47768081/102999386-40bf5e80-456c-11eb-86fd-767d3c722f8d.png)

  



```python
unknwon_person = fr.load_image_file("/gdrive/My Drive/colab/unknown.jpg")

top, right, bottom, left = fr.face_locations(unknwon_person)[0]
unknown_face = unknwon_person[top:bottom, left:right]

plt.title("unknown_face")
plt.imshow(unknown_face)
plt.show()
```

- 기존 리스트(known_person_list)에 없는 새로운 파일을 열고 얼굴 좌표를 알아내서 잘라낸다

- unknown_face 이라는 타이틀을 붙여서 표시한 후 이미지 출력

- 출력 화면 :

  ![image-20201223200742626](https://user-images.githubusercontent.com/47768081/102999434-56348880-456c-11eb-9cf8-9df3d1036e33.png)



```python
# unknown_person_face를 인코딩
enc_unknown_face = fr.face_encodings(unknown_face)

# 화면에 표시해보면 다음과 같다
plt.imshow(enc_unknown_face)
plt.show()
```

- **face_encoding 설명**

![image-20201223210106931](https://user-images.githubusercontent.com/47768081/102999444-57fe4c00-456c-11eb-938a-ef0d41e25036.png)

[참조] https://ukayzm.github.io/unknown-face-classifier/

1. 그림에서 얼굴이 있는 영역을 알아낸다 (face location)
2. 얼굴 영역에서 눈, 코, 입 등 68개의 주요 좌표를 추출한다 (facial landmarks)
3. **68개의 좌표를 128개의 숫자로 변환한다 (face encoding)**



```python
for face in known_face_list:

  enc_known_face = fr.face_encodings(face)

  distance = fr.face_distance(enc_known_face, enc_unknown_face[0])

  plt.title("distance: " + str(distance))
  plt.imshow(face)
  plt.show()
```

- 등록된 얼굴리스트를 비교하는 부분

  - 등록된 얼굴을 128-dimensional face 인코딩 후

  - 등록된 얼굴과 새로운 얼굴의 distance를 얻기

  - distance 수치를 포함한 얼굴 출력

    -> 출력 화면 : 

    ![image-20201223210939256](https://user-images.githubusercontent.com/47768081/102999448-59c80f80-456c-11eb-97d5-d5d5bba23f35.png)

    * 일반적으로 distance 가 0.6 이상이면 타인이라고 볼 수 있지만 비슷한 사람은 0.5대 distance가 나오기도 한다
    * distance값이 가장 작은 2번째 person이 unknown이미지의 사람과 동일인이라고 볼 수 있다





### 전체 코드

```python
from google.colab import drive
drive.mount('/gdrive')

pip install face_recognition

import cv2, os
import face_recognition as fr
from IPython.display import Image, display
from matplotlib import pyplot as plt

image_path = "/gdrive/My Drive/colab/bts.jpg"

image = fr.load_image_file(image_path)
face_locations = fr.face_locations(image)

for(top, right, bottom, left) in face_locations:
  cv2.rectangle(image, (left, top), (right, bottom), (0,255,0), 3)


# 이미지 버퍼 출력
plt.rcParams["figure.figsize"] = (16,16)
plt.imshow(image)
plt.show()


plt.rcParams["figure.figsize"] = (1,1)

# 이미지 파일을 로드하여 known_person_list 리스트 생성
known_person_list = []
known_person_list.append(fr.load_image_file("/gdrive/My Drive/colab/person1.jpg"))
known_person_list.append(fr.load_image_file("/gdrive/My Drive/colab/person2.jpg"))
known_person_list.append(fr.load_image_file("/gdrive/My Drive/colab/person3.jpg"))
known_person_list.append(fr.load_image_file("/gdrive/My Drive/colab/person4.jpg"))

# 얼굴을 인식을 하여 감지된 부분을 잘라낸 다음 known_face_list에 저장
known_face_list = []
for person in known_person_list:

  # 얼굴 좌표를 알아내서 잘라낸다
  top, right, bottom, left = fr.face_locations(person)[0]
  face_image = person[top:bottom, left:right]

  # known_face_list에 잘라낸 face_image를 저장
  known_face_list.append(face_image)


# known_face_list에 저장된 얼굴들 출력
for face in known_face_list:
  plt.imshow(face)
  plt.show()
    
# 기존 리스트에 없는 새로운 파일을 열어서
unknwon_person = fr.load_image_file("/gdrive/My Drive/colab/unknown.jpg")

# 얼굴 좌표를 알아내서 잘라낸다
top, right, bottom, left = fr.face_locations(unknwon_person)[0]
unknown_face = unknwon_person[top:bottom, left:right]

# unknown_face 이라는 타이틀을 붙여서 표시
plt.title("unknown_face")
plt.imshow(unknown_face)
plt.show()


# unknown_person_face를 인코딩
enc_unknown_face = fr.face_encodings(unknown_face)

# 화면에 표시해보면 다음과 같다
plt.imshow(enc_unknown_face)
plt.show()

# 등록된 얼굴리스트를 비교
for face in known_face_list:

  # 등록된 얼굴을 128-dimensional face 인코딩
  enc_known_face = fr.face_encodings(face)

  # 등록된 얼굴과 새로운 얼굴의 distance를 얻기
  distance = fr.face_distance(enc_known_face, enc_unknown_face[0])

  # distance 수치를 포함한 얼굴 출력
  plt.title("distance: " + str(distance))
  plt.imshow(face)
  plt.show()

```









Q.  **아바타**나 **웹툰 이미지**도 사람으로 인식하나?

Test 결과 => **얼굴로 인식함!**

![image-20201223213424737](https://user-images.githubusercontent.com/47768081/102999534-82e8a000-456c-11eb-8c10-37296a739c7d.png)



![image-20201223213635226](https://user-images.githubusercontent.com/47768081/102999538-8419cd00-456c-11eb-9d4f-5bde1f2c39b1.png)



하지만 다른 멤버가 더 동일인물이라고 나온다 (SM 보고있나요...?)



-> 그렇다면 웹툰과 드라마 캐스팅은 얼마나 잘됬는지??



![image-20201223214102185](https://user-images.githubusercontent.com/47768081/102999573-94ca4300-456c-11eb-9229-262d287d8d6f.png)

결론 : 웹툰 그림은 얼굴 인식은 하지만 유사도 체크는 어려운 걸로!





> **실습 후 느낀점** 
>
> 얼굴인식 PJT를 따라하고 공부해보고, 실습을 하면서 데이터 시각화, 파이썬 프로그래밍 언어, 4차 산업기술 등에 대해 관심이 생기게 되었다.
>
> 이후에 이 기술들을 활용해 프로젝트를 해봐야겠다.
