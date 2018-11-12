---
title: "Tensorfboard usage"
layout: post
date: 2018-11-12 10:25222255555555555555
tag:
- tensorflow
- notes
category: blog
author: jaekwang
description: tensorboard usage
---

## How to use tensorboard
 - [code 예제는 해당링크에 예제가 있다.](https://github.com/anjaekwang/tensorflow_example/blob/master/tensorboard_usage.py)


### weight 와 bias에 summary 작성, 틀
~~~
with tf.name_scope("layer1") as scope: #tensorboard 의 도형 만들어냄.
    w1 = tf.get_variable("w1", shape=[3,3,1,5],initializer=tf.contrib.layers.xavier_initializer())
    b1 = tf.get_variable("b1", shape=[5],initializer=tf.contrib.layers.xavier_initializer())

    conv1 = tf.nn.relu(tf.nn.conv2d(X, w1, strides=[1, 1, 1, 1], padding='SAME') + b1)

    w1_hist = tf.summary.histogram("weights1", w1) #histogram이 아닌 scalar 경우 tf.summary.scalar
    b1_hist = tf.summary.histogram("biases1", b1)
    layer1_hist = tf.summary.histogram("layer1", conv1)
~~~

-  즉 with ..as scope 는 도형을 만들어내고 summary 써야 그래프 나옴

### Session을 열고 summary를 log파일로 작성하기
~~~
  1) merged_summary = tf.summary.merge_all() #summary들을 합침
  2) writer = tf.summary.FileWriter(summ_dir) #log파일생성
  3) writer.add_graph(sess.graph)  # Show the graph # 그래프 그려주고
   # 1~3은 연속적으로 이뤄짐 세션여는 순간 .. 즉 작성시점이 데이터를 기록하기 전에 작성되야ㅕ함
  4)summary = sess.run(merged_summary ) -> train 돌릴때 같이
  5) writer.add_summary(summary, global_step=epoch)
   # 학습함에따라 변화대는 log를 보는것이므로 학습되어지는 라인에 작성해줘야함 global_step 은 그 때마다 log를 기록해줌
~~~


## log 보는 방법
  cmd 창에 tensorboard --logdir=경로   
   ()오류시 : cd로 log파일 상위폴더로 이동후 tensorboard --logdir=./log_file)
 열린후에 localhost:6006 혹은 터미널에서 뜬 주소로 접근하면 볼수있다.
