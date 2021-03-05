# 자료구조와 알고리즘

## 자료구조
- 서비스, 어플리케이션에 필요한 데이터를 메모리에 어떻게 구조적으로 잘 정리해서 담아두고 관리하여 최종적으로 가장 효율적인 방식으로 필요한 데이터에 빠르게 접근하고 수정, 삽입, 삭제할 수 있도록 도와준다.
- 서비스에 클라이언트한테 데이터를 제공하거나, 어플리케이션에서 사용자에게 필요한 데이터를 보여주거나 수정할 때 효율적으로 일을 처리하기 위해서는 기능에 적합한 알맞은 자료구조를 쓰는 것이 정말 중요하다. 이는 어떤 자료구조를 쓰냐에 따라서 사용자가 원하는 기능을 수행하는 데 0.2초가 걸릴 수도 2초가 걸릴 수도 있기 때문이다.
- 자료구조의 종류로는 배열, 단일 연결 List, 스택, Hash table 등이 있다.
- 중점적으로 생각해야할 부분으로는,
1. 자료구조 안에 있는 데이터의 순서가 보장이 되는지?
2. 중복된 데이터가 들어갈 수 있는지?
3. 검색할 때 얼마나 효율적인지?
4. 우리가 원하는 기능에 따라서 수정할 때 얼마나 효율적인지?

## 알고리즘
- 제한된 공간과 시간 안에서 데이터를 어떻게 처리할지를 정의해놓은 로직인데 즉, 주어진 input으로 정의된 계산을 수행한 다음에 output을 내는 것을 말한다.
- BigO: 동일한 알고리즘의 로직으로 input의 사이즈가 점점 커질수록 시간이 얼마나 더 많이 걸리느냐를 정의한 시간복잡도를 나타낼 수 있는 방법이다.
- 주어진 데이터를 검색하거나 정렬 또는 총점을 구하는 등의 다양한 계산을 할 수 있는 것을 말한다.
- 중점적으로 생각해야할 부분으로는,
1. input 사이즈가 커질 수록 BigO가 어떻게 변화하는지?
2. 공간과 시간의 복잡도는 어떤지?
3. 어떤 자료구조를 이용해서 이 알고리즘을 쓰는 게 좋은지?
- 제일 좋은 알고리즘은 제공된 데이터를 정말 작은 공간과 빠른 시간 안에서 효율적으로 처리할 수 있는 것이 가장 좋다.

### 참고
[드림코딩 by 엘리 유튜브](https://youtu.be/okHGRlgR8ps)