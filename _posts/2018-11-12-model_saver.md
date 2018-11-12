---
title: "tensorflow model 가중치 저장"
layout: post
date: 2018-11-12 10:25
tag:
- tensorflow
- notes
category: blog
author: jaekwang
description: saver를 이용한 model의 가중치 저장.
---

## How to use tensorflow Saver

### 학습한 가중치 저장
 - session이 열린 상태에서 session을 넘겨 줘야한다. , save의 마지막경로는 없는 파일 이여야 한다
   즉, 마지막 이름이 파일의 이름.
 1. saver = tf.train.Saver() #saver 객체를 만들어준다.
 2. ckpt 기존 파일을 load 한다
 3. saver.save(sess, checkpoint_dir, global_step = epoch) 학습을 시키는 반복문안에 원하는 step 간격만큼 넘기면 그때 마다 가중치 저장, ex)epoch을 주면 epoch 마다 저장.

### meta 파일(가중치 저장한) load
 * 학습한 가중치 저장의 2)의 내용.  
  1. ckpt = tf.train.get_checkpoint_state(checkpoint_dir)
     save할때 checkpoint state protocol buffer를 checkpoint 파일에 저장하는데
     checkpoint stae protocol buffer에는
     * model_chechpoint_path :  가장 최근 가중치 파일의 path 정보
        - 이용 ckpt.model_checkpoint_paths -> return saved/train2-9   
     * all_model_checkpoint_paths : 모든 path
        - 이용 ckpt.all_model_checkpoint_paths -> return [saved/train2-5, saved/train2-6, saved/train2-7]
 2.  _

 ```
  if ckpt and ckpt.model_checkpoint_path: #가장최근에 ckpt파일에 접근한다.
    ckpt_name = os.path.basename(ckpt.model_checkpoint_path) #입력받은 경로의 기본 이름을 반환한다.
    self.saver.restore(self.sess, os.path.join(checkpoint_dir, ckpt_name)) #dir : meta file
    return True

  else:
    return False
 ```
