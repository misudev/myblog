---
layout: post
title: "[Algorithm] 기능 개발"
category: Algorithm
tags: [Algorithm,java]
---

[프로그래머스 : 문제링크](https://programmers.co.kr/learn/courses/30/lessons/42586)
## 문제
>프로그래머스 팀에서는 기능 개선 작업을 수행 중입니다. 각 기능은 진도가 100%일 때 서비스에 반영할 수 있습니다.  
>또, 각 기능의 개발속도는 모두 다르기 때문에 뒤에 있는 기능이 앞에 있는 기능보다 먼저 개발될 수 있고, 이때 뒤에 있는 기능은 앞에 있는 기능이 배포될 때 함께 배포됩니다.  
>먼저 배포되어야 하는 순서대로 작업의 진도가 적힌 정수 배열 progresses와 각 작업의 개발 속도가 적힌 정수 배열 speeds가 주어질 각 배포마다 몇 개의 기능이 배포되는지를 return 하도록 solution 함수를 완성하세요.


## 조건
>작업의 개수(progresses, speeds배열의 길이)는 100개 이하입니다.  
>작업 진도는 100 미만의 자연수입니다.  
>작업 속도는 100 이하의 자연수입니다.  
>배포는 하루에 한 번만 할 수 있으며, 하루의 끝에 이루어진다고 가정합니다.   예를 들어 진도율이 95%인 작업의 개발 속도가 하루에 4%라면 배포는 2일 뒤에 이루어집니다.

------------------------------

## 풀이코드
```java
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.Queue;
class Solution {
    public int[] solution(int[] progresses, int[] speeds) {
        ArrayList<Integer> arrayList = new ArrayList<Integer>();
        Queue<Integer> queue = new LinkedList<>();

        for(int i = 0; i < speeds.length; i++){
            queue.add((int)Math.ceil((100 - progresses[i])/speeds[i]));
        }

        while(!queue.isEmpty()){
            int count = 1;
            int poll = queue.poll();
            while(!queue.isEmpty() && poll >= queue.peek()){
                queue.poll();
                count ++;
            }
            arrayList.add(count);
        }

        int[] answer = new int[arrayList.size()];
        for(int i = 0; i < answer.length; i++){
            answer[i] = arrayList.get(i);
        }
        return  answer;
    }
}
```

먼저 작업마다 남은 시간을 계산해주고 이를 queue에 담아준다.   
남은 작업시간은 다음과 같다.
```
Math.ceil((100 - progresses[i])/speeds[i])
// 나머지가 있으면 무조건 올림 해준다.
```
그리고 queue가 빌 때 까지 다음 작업을 반복한다.
1. count는 1로 시작한다 (queue에 값이 남아있으므로.)
2. queue에 맨 처음값을 꺼낸다.
3. 현재 queue의 처음 값이 2에서 빼준 값보다 작은 경우 값을 꺼내고 count 증가시킨다.
4. 3을 맨 처음 꺼낸값보다 큰 값을 만날 때 까지 반복한다.
5. arrayList에 배포한 갯수 count를 추가한다.

이때 주의할 점은 4번에서 맨 처음 꺼낸값과 현재 값이 같은 경우에도 queue에서 값을 꺼내야 하는 점이다.  
