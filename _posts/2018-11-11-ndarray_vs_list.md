---
title: "ndarray vs list 차이점"
layout: post
date: 2018-11-10 18:06
tag:
- python
- notes
category: blog
author: jaekwang
description: numpy array와 기본 array의 차이점.
---

##ndarray vs. list 차이점

### 차이점
* list의 element는 가변적이나 ndarray의 element는 type과 shape이 같아야한다.
* 내가 자주사용하는[ , , ]방식의 슬라이싱은 ndarray만 가능하다.

### Append 방식
* **list**<br/><br/>
 list.append(element) : 반환형 없이 모양그대로 들어간다<br/>
 ex) [1,2,[1,2,3]] <br/><br/>
 x = []<br/>
 x.append(추가할 원소)

* **ndarray**<br/><br/>
  X = np.append(X,원소) : 반환형으로 결과가 나오며, flattened 된 array가 들어간다
  그래서 **해당 원소가 shape이 같다면** 그냥 flatten 하게 넣은뒤 원하는 shape으로
  reshape 해준다(reshape결과 또한 반환되어 나온다.)<br/><br/>
**이때 append할 element들이 shape과 type이 같아야한다.**<br/>
x = np.array([])<br/>
x = np.append(x, 추가할 원소)

## Tip

* nparray -> list  : list(nparray)
* nparray 슬라이싱 해도 nparray
* append 과정에서 shape 다르다면 reshape을 잘 이용해 문제해결 하자.
