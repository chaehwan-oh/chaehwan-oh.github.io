---
layout: post
subclass: post
title: "binary search 와 lower bound"
date: 2019-03-28
tags: [problem-solving, algorithm, binary-search, lower-bound, c++]
excerpt: "algorithm"
disqus: True
---

알고리즘 문제해결전략 '너드인가 아닌가2' 문제에서 C++ STL lower_bound() 함수을 활용해 문제를 해결하는 방법이 소개되어 있다. 관련해서 lower bound 와 binary search 에 대해 정리해봤다.

간단하게 먼저 binary search 에 대해 살펴보자. lower bound가 binary search 의 원리를 활용하기 때문에 복습을 위해 구현해보았다.

## binary search

```cpp
#include <iostream>
#include <cassert>

using namespace std;

int recursive_binary_search(int search_space[], int begin, int end, int key)
{
    // base case: there is no more search space
    if (begin > end)
    {
        return -1;
    }
    int middle = (begin + end) / 2;
    // base case: when the key is found(search_space[middle] == key)
    if (search_space[middle] == key)
    {
        return middle;
    }
    // recursive case: search_space[middle] < key
    else if (search_space[middle] < key)
    {
        return recursive_binary_search(search_space, middle + 1, end, key);
    }
    // recursive case: search_space[middle] > key
    else
    {
        return recursive_binary_search(search_space, begin, middle - 1, key);
    }
}

int iterative_binary_search(int search_space[], int begin, int end, int key)
{
    while (begin <= end)
    {
        int middle = (begin + end) / 2;
        if (search_space[middle] == key)
        {
            return middle;
        }
        else if (search_space[middle] < key)
        {
            begin = middle + 1;
        }
        else
        {
            end = middle - 1;
        }
    }
    return -1;
}

int main()
{
    int test_cases[5][6] = {
        {1, 2, 3, 5, 8, 9},
        {3, 5, 6, 8, 9, 10},
        {-1, 0, 2, 3, 4, 5},
        {-3, 0, 3, 10, 11, 14},
        {-5, 0, 5, 10, 15, 20}
    };
    int label[5] = {2, 0, 3, 2, -1}; // label is the key index when the key is 3
    int start = 0;
    int last = 5;
    int key = 3;
    for (int i = 0; i < 5; i++)
    {
        cout << recursive_binary_search(test_cases[i], start, last, key) << endl;
        cout << iterative_binary_search(test_cases[i], start, last, key) << endl;
        assert (recursive_binary_search(test_cases[i], start, last, key) == label[i]);
        assert (iterative_binary_search(test_cases[i], start, last, key) == label[i]);
    }
    return 0;
}
```

결국 중요한 것은 종료 조건, recursion으로 말하면 base case를 어떻게 설정할 것이고, 탐색 범위를 줄여 나가기 위해 변수를 어떻게 변경할 것인가를 정하는 것이 핵심이다.

(<생각하는 프로그래밍>이라는 책에서는 이분 탐색도 제대로 구현하기란 쉽지 않다고 소개하고 있다. 그 이유는 다음 기회에 살펴보도록 하자.)

다음은 이어서 lower bound다.

## lower bound

요구사항)

- n개로 이루어진 정수 집합에서 원하는 수 k 이상인 수가 처음으로 등장하는 위치
  를 찾으시오.
  (단, 입력되는 집합은 오름차순으로 정렬되어 있으며, 같은 수가 여러 개 존재할 수 있다.)
- 찾고자 하는 원소의 위치를 출력한다. 만약 모든 원소가 k보다 작으면 n+1을 출력한다.

```cpp
#include <iostream>
#include <vector>
#include <cassert>

using namespace std;

int get_lower_bound(const vector<int>& search_space,
                    int begin, int end, int key)
{
    while (begin < end)
    {
        int middle = (begin + end) / 2;
        if (search_space[middle] < key)
        {
            begin = middle + 1;
        }
        else
        {
            end = middle;
        }
    }
    return end + 1;
}

int main()
{
    vector<vector<int>> test_cases{
        {1, 3, 5, 7, 7},
        {1, 2, 3, 5, 7, 9, 11, 15},
        {1, 2, 3, 4, 5}
    };
    vector<int> keys{
        {7, 6, 7}
    };
    vector<int> labels{
        {4, 5, 6}
    };
    for (int i = 0; i < test_cases.size(); i++)
    {
        cout << get_lower_bound(test_cases[i], 0,
                                test_cases[i].size(), keys[i]) << endl;
        assert (get_lower_bound(test_cases[i], 0,
                                test_cases[i].size(), keys[i]) == labels[i]);
    }
    return 0;
}
```

lower bound는 이분 탐색과 같이 오름차순으로 정렬되어 있다는 전제가 먼저 필요하다. 그러한 전제 하에서 찾고자 하는 수 key 이상인 값이 처음 등장하는 위치를 구하는 문제다.

이분 탐색과 같은 원리로 탐색 공간을 줄여나갈 수 있다.

```
{1, 2, 3, 5, 7, 9, 11, 15} / key: 6
```

위의 경우를 예로 알고리즘을 적용해보도록 하자.

1. 먼저 시작과 끝을 정한다.

이분 탐색과 같이 시작과 끝을 정해주어야 한다. 이때 마지막 위치는 n+1 이 되도록 한다. 그 이유는 모든 수가 key 보다 작을 때 n+1을 출력해주어야 하기 때문이다.

2. 중간값을 정한다.

여기서는 (0 + 8) / 2 = 4 로 4가 된다. 4번째 인덱스인 7이 중간 지점이 된다.

3. 중간 위치의 값과 key 값을 비교한다.

중간 위치의 값인 7 과 key 값인 6을 비교한다.

4. 탐색 범위를 조정해준다.

중간 위치의 값이 key 값 6보다 크므로 중간 위치의 값을 포함한 [0:4] 로 범위를 조정해준다.
이분 탐색이었다면 0:3 으로 범위를 줄였을 것이다. 하지만 중간 위치의 값 자체도 후보가 될 수 있기 때문에 포함하고 탐색 범위를 조정한다.

5. 중간값을 정하고 탐색 범위를 조정하는 과정을 다시 진행한다.

다음 중간 위치는 2이고 해당하는 값은 3이다. 하지만 이번에는 key보다 더 작으므로 [3:4] 로 범위를 조정해준다. key보다 더 작은 값은 후보가 될 수 없으므로 배제하는 것이다.

이러한 방식으로 계속 진행하면 결국 인덱스가 4인 위치에 key 값 이상인 최소의 요소가 나타나는 것을 알 수 있다. 1 ~ n 이 기준이므로 출력하는 값은 5가 된다.

**알고리즘 자체를 외우기보다 왜 어떻게 탐색 범위를 줄여갈 수 있는지 생각하면 이해가 조금 더 수월할 것 같다.**
