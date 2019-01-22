---
layout: post
title: "[Algorithm] 다리를 지나는 트럭"
category: Algorithm
tags: [Algorithm,java]
---

[프로그래머스 : 문제링크](https://programmers.co.kr/learn/courses/30/lessons/42583)
## 문제
>트럭 여러 대가 강을 가로지르는 일 차선 다리를 정해진 순으로 건너려 합니다.
>모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 알아내야 합니다.
>트럭은 1초에 1만큼 움직이며, 다리 길이는 bridge_length이고 다리는 무게 weight까지 견딥니다.
>※ 트럭이 다리에 완전히 오르지 않은 경우, 이 트럭의 무게는 고려하지 않습니다.


## 조건
>bridge_length는 1 이상 10,000 이하입니다.
>weight는 1 이상 10,000 이하입니다.
>truck_weights의 길이는 1 이상 10,000 이하입니다.
>모든 트럭의 무게는 1 이상 weight 이하입니다.

------------------------------

## 풀이코드
```java
import java.util.LinkedList;
import java.util.Queue;
class Solution {
    public int solution(int bridge_length, int weight, int[] truck_weights) {
        Queue<Truck> bridge = new LinkedList<Truck>();
        Truck truck= new Truck(truck_weights[0], bridge_length -1);
        bridge.add(truck);
        // 1초 일때로 초기화
        int total = truck_weights[0];
        int time = 1;
        int index = 1;
        // 다리에 트럭이 없을때까지 반복
        while(!bridge.isEmpty()){
            time += 1;
            if(bridge.peek().getLength()==0){
                Truck t = bridge.poll();
                total -= t.getWeight();
            }
            if(index < truck_weights.length){
                if( (bridge.size() < bridge_length) &&( total + truck_weights[index] <= weight)){
                    truck = new Truck(truck_weights[index++], bridge_length);
                    bridge.add(truck);
                    total += truck.getWeight();
                }
            }
            for(Truck t : bridge){
                t.minusLength();
            }
        }
        return time;
    }
}

class Truck{
    private int weight;
    private int length;
    public Truck(int weight, int bridge_length){
        this.weight = weight;
        this.length = bridge_length;
    }

    public int getWeight() {
        return weight;
    }

    public int getLength() {
        return length;
    }

    public void minusLength(){
        length--;
    }
}
```
Truck 클래스를 만들었고, Truck은 weight과 length를 가진다.
- length는 현재 남은 다리의 길이를 의미한다.  
- weight는 트럭의 무게를 의미한다.

이러한 Truck 타입의 Queue bridge를 선언한다. 이 bridge(Queue)에는 현재 다리위에 있는 트럭들이 들어있다.    
처음 1초일 때로 time, bridge, total, index의 값을 설정한다. total은 다리위의 트럭들의 총 무게, index는 다음에 다리에 올라올 트럭의 순서를 가르킨다.  
이제 다음과 같은 작업이 실행된다.
1. 시간을 1초 증가시킨다.
2. 다리를 다 건넌 트럭을 bridge에서 꺼내고 total에서 꺼낸 트럭의 무게를 빼준다.
3. 다리에 올라갈 트럭이 있고, 다리가 꽉 차있지 않고, 다리의 한도 무게보다 (total + 다음 트럭의 무게)가 같거나 작으면 다리에 트럭을 추가한다.  
4. 다리에 올라간 트럭의 남은 거리를 -1 해준다.  

bridge가 빌 때 까지 위의 작업을 반복한다.  
