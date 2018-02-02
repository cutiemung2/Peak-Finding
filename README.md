# Peak Finding
</hr>


## Peak이란?

다음과 같은 1차원 배열이 있다고 하자.

![1d_array](https://user-images.githubusercontent.com/35156601/35719571-5f80741e-082d-11e8-9da1-7594e98ab093.JPG)


그러면 Peak은 다음을 뜻한다.
 b>=a 이고 b>=c 이면 b는 peak이다.
양 끝에 존재하는 요소 i의 경우에는 
i>=h 이면 peak이다.

위의 Peak의 규칙에서 알 수 있듯이 하나의 배열안에는 두개 이상의 Peak이 존재 할 수 있다.


## 목표

배열이 주어졌을 때, 존재하는 하나의 Peak값을 찾아내는 것이다. 여러개라도 하나만 찾으면 된다.
최댓값을 찾는문제는 아니다. 그저 양 옆의 값이 자신보다 크지않다면 그 값을 Peak으로 선택하면 된다.
입력은 1차원과 2차원 배열로 주어진다.


## 1차원 배열에서의 Peak Finding

### 1. 직관적인 방법

비교적 쉽게 생각할 수 있는 알고리즘이다.
배열의 왼쪽에서 시작하여 비교를 통해 오른쪽으로 한칸씩 나아가는 방법이다.
최악의 경우 n번의 비교연산을 진행하게 되어 Θ(n)의 시간복잡도를 보인다.

본인이 작성한 코드는 다음과 같다.
```
//# python3.6
def straightforward_1D(self,input_numbers):
    length = len(input_numbers)
    for idx in range(length-1):
        if input_numbers[idx]>=input_numbers[idx+1]:
            return input_numbers[idx]
        
    return input_numbers[length-1]
```


End with an example of getting some data out of the system or using it for a little demo


### 2. 좀 더 효율적인 방법 - Divide and Conquer

더 빠른 알고리즘을 원한다면 조금은 더 복잡한 방법이 필요하다.

그 중 하나의 방법으로 이진 탐색을 사용 할 수 있는데, 배열을 반으로 쪼개가며 탐색 범위를 좁혀 나가기 때문에 "Divide and Conquer" 방식이라고 부른다. 

알고리즘은 다음과 같다.
· 먼저, 탐색 대상인 배열의 정 가운데를 선택한다.
· 선택된 값과 인접한 양 옆의 값을 비교한다.
· 왼쪽 값이 더 크다면 배열 정 중앙 기준으로 왼쪽에 있는 부분을 새로운 탐색대상으로 설정하여 반복한다.
· 그렇지 않고 오른쪽 값이 더 크다면 배열 정 중앙 기준으로 오른쪽에 있는 부분을 새로운 탐색대상으로 설정한다.
· 둘 다 아니라면, 즉 양옆에 자신보다 큰 값이 없다면 해당 값을 Peak으로 설정하고 탐색을 마친다.

본인이 작성한 코드는 다음과 같다.

```
def div_and_conq_1D(self, input_numbers):
    length = len(input_numbers)
    first, last = 0, length-1 # first, last = 탐색대상의 처음과 마지막 인덱스
      
    while True:#first != last:
        mid = (first + last)//2
        if mid>first and input_numbers[mid-1] > input_numbers[mid]: #mid>0
            last = mid-1
        elif mid<last and input_numbers[mid+1] > input_numbers[mid]: #mid<length-1
            first = mid+1
        else:
            return input_numbers[mid]
    return input_numbers[first]
```

이 알고리즘은 이전에 했던 직관적인 방법보다 속도면에서 효율이 뛰어나다. 
직관적인 방법은 한번 비교할 때마다 탐색대상이 1개씩 줄어들었는데, 이 경우에는 한번 비교할 때마다 탐색 대상이 반으로 줄어드니
데이터 양이 많을수록 빠른 속도를 보여준다.  Θ(log(n))의 시간복잡도를 가진다.

![1d](https://user-images.githubusercontent.com/35156601/35719949-03303ecc-082f-11e8-8ff7-577357434b2e.jpg)



## 2차원 배열에서의 Peak Finding

2차원에서는 1차원 규칙이 확장된다.
아래와 같은 2차원 배열이 존재할 때, a>= b, c, d, e이면 a는 Peak이다.

![2dpeak](https://user-images.githubusercontent.com/35156601/35719978-24cc8aa4-082f-11e8-8119-bf6dda2acfd3.JPG)


### 1. 직관적인 방법 (Greedy Ascent)
직관적인 방법으로는 첫번째 인덱스부터 마지막인덱스까지 하나하나 비교하는 방법이 있다.
또, 현재 상태에서 가장 큰 값을 쫒아가는 Greedy한 방법도 있을 수 있다.
이 외에도 생각할 수 있는 방법은 여러개가 있는데, 여기서는 그 중 하나를 사용할 것이다.


알고리즘 순서는 다음과 같다.
· 배열상에서 임의의 위치를 잡는다. 인접한 셀 4개 중에서 자신보다 큰 값이 발견되면 그 곳으로 위치를 옮긴다.
· 셀 4개중에 자신보다 큰 값이 없으면 그 값이 Peak이다.

배열의 크기가 n x m이라면 최악의 경우 n x m번의 비교과정을 거쳐야하므로 Θ(nm)의 시간복잡도를 보인다.

본인이 작성한 코드다.
```
def straightforward_2D(self,input_numbers):
    n, m = len(input_numbers), len(input_numbers[0]) # n,m = #row, #col
    row, col = n//2, m//2
    
    for _ in range(m*n):
        if row>0 and input_numbers[row-1][col] > input_numbers[row][col]:
            row = row-1
        elif row<n-1 and input_numbers[row+1][col] > input_numbers[row][col]:
            row = row+1
        elif col>0 and input_numbers[row][col-1] > input_numbers[row][col]:
            col = col-1
        elif col<m-1 and input_numbers[row][col+1] > input_numbers[row][col]:
            col = col+1
        else:    
            return input_numbers[row][col]
    print("logic error!")
```


### 2. 좀 더 효율적인 방법 - Divide and Conquer

2차원 배열에서 역시, 1차원과 비슷한 방식으로 연산횟수를 대폭 줄일 수 있다.
1차원에서 사용했던 Divide and Conquer 알고리즘을 살짝만 바꾸어 2차원에 적용시켜보겠다.

n개의 행(row)과 m개의 열(col)을 가지는 배열이 있다고 하자.

우선, 가운데에 있는 열을 선택한다. m/2번째 열이 되겠다.


## Deployment

Add additional notes about how to deploy this on a live system

## Built With

* [Dropwizard](http://www.dropwizard.io/1.0.2/docs/) - The web framework used
* [Maven](https://maven.apache.org/) - Dependency Management
* [ROME](https://rometools.github.io/rome/) - Used to generate RSS Feeds

## Contributing

Please read [CONTRIBUTING.md](https://gist.github.com/PurpleBooth/b24679402957c63ec426) for details on our code of conduct, and the process for submitting pull requests to us.

## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/your/project/tags). 

## Authors

* **Billie Thompson** - *Initial work* - [PurpleBooth](https://github.com/PurpleBooth)

See also the list of [contributors](https://github.com/your/project/contributors) who participated in this project.

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* Hat tip to anyone who's code was used
* Inspiration
* etc

