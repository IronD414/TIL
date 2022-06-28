## 2022.06.27
# 배열에 대한 이해

## Duplicate Zeros
Given a fixed-length integer array arr, duplicate each occurrence of zero, shifting the remaining elements to the right.

Note that elements beyond the length of the original array are not written. Do the above modifications to the input array in place and do not return anything.

 

> Example 1:
>>Input: arr = [1,0,2,3,0,4,5,0] 
>>
>>Output: [1,0,0,2,3,0,0,4]
>
>Explanation: After calling your function, the input array is modified to: [1,0,0,2,3,0,0,4]
>
>Example 2:
>
>>Input: arr = [1,2,3]
>>
>>Output: [1,2,3]
>
>Explanation: After calling your function, the input array is modified to: [1,2,3]
> 
>Constraints:
>>
>>1 <= arr.length <= 104
>>
>>0 <= arr[i] <= 9
>
주어진 배열에 대해 0은 하나씩 추가되게 하고, 배열의 길이를 유지하기 위해 그만큼 뒤의 요소들이 제거되게 하도록 만드는 문제다.

문제를 해결하기 위해 
    
    for i in range(len(arr)):
        if arr[i] == 0:
            for j in reversed(range()):

이런식으로 작성하려 했는데 처음에 range() 안의 범위를
잘못 잡아서 - 뒤쪽에서부터 배열을 하나씩 뒤로 당기는건데 시작 범위가 i에 의해 바뀌게 설정해버렸다 - 계속 오류가 났다. 

뒤쪽부터 당기는 걸 시작해야 하는데 맨 뒤의 index는 len(arr)-1로 정해져있기 때문에 range( , len(arr)-1)이 돼야 한다. 이 사실을 알고 나니 빠르게 풀렸다.

<hr/>

## Merge Sorted Array

You are given two integer arrays nums1 and nums2, sorted in non-decreasing order, and two integers m and n, representing the number of elements in nums1 and nums2 respectively.

Merge nums1 and nums2 into a single array sorted in non-decreasing order.

The final sorted array should not be returned by the function, but instead be stored inside the array nums1. To accommodate this, nums1 has a length of m + n, where the first m elements denote the elements that should be merged, and the last n elements are set to 0 and should be ignored. nums2 has a length of n.

>Example 1:
>>Input: nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
>>
>>Output: [1,2,2,3,5,6]
>>
>Explanation: The arrays we are merging are [1,2,3] and [2,5,6].
>
>The result of the merge is [1,2,2,3,5,6] with the underlined elements coming from nums1.

>Example 2:
>
>>Input: nums1 = [1], m = 1, nums2 = [], n = 0
>>
>>Output: [1]
>
>Explanation: The arrays we are merging are [1] and [].
>The result of the merge is [1].

>Example 3:
>
>>Input: nums1 = [0], m = 0, nums2 = [1], n = 1
>>
>>Output: [1]
>
>Explanation: The arrays we are merging are [] and [1].
>The result of the merge is [1].
>Note that because m = 0, there are no elements in nums1. The 0 is only there to ensure the merge result can fit in nums1.
 

>Constraints:
>
>>nums1.length == m + n
>>
>>nums2.length == n
>>
>>0 <= m, n <= 200
>>
>>1 <= m + n <= 200
>>
>>-109 <= nums1[i], nums2[j] <= 109
 

>Follow up: Can you come up with an algorithm that runs in O(m + n) time?

이 문제가 굉장히 쉽다고 생각했다가 큰 코를 다쳤다.
'아~ nums1의 위치는 x1, nums2의 위치는 x2로 가리키면서 두 개를 비교하며 더 적은 쪽을 새로운 배열에 append 해서 return하면 되겠네~'

이 생각은 물론 맞는 생각이지만, edge cases를 전혀 고려하지 않은 단편적인 생각이다.

만약 m, n이 0이 아니라는 조건이 있다면 위처럼 작성하면 된다.

또한, nums1과 nums2의 요소들의 값이 양수로 시작한다면 문제는 쉬워진다.

하지만 그렇지 않고, 나는 그걸 고려하지 않고 코드를 작성했다. 그랬더니 


    input
        num1: [0,0,0,3,0,0,0]
        num2: [-2,-1,5]

와 같은 edge case가 아닌 경우에서조차 오류가 떴으며(내가 edge case를 처리한답시고 first element 값이 0인 경우에 예외 처리를 했기 때문이었다)


    input
        num1: [1]
        num2: []

와 같은 case는 바로 index out of range가 나버렸다.

그래서 edge case의 기준은 m과 n의 값이 0인 경우로 해야겠다고 생각했다.

나의 답변 코드는 다음과 같다.


    class Solution(object):
    def merge(self, nums1, m, nums2, n):
        """
        :type nums1: List[int]
        :type m: int
        :type nums2: List[int]
        :type n: int
        :rtype: None Do not return anything, modify nums1 in-place instead.
        """
        nums3 = []        
        x1 = 0
        x2 = 0
        
        if m == 0:
            for i in range(n):
                nums1[i] = nums2[i]
        elif n != 0:
            for q in range(m+n):
                if x1 == m:
                    nums3.append(nums2[x2])
                    x2 += 1
                elif x2 == n:
                    nums3.append(nums1[x1])
                    x1 += 1
                elif nums1[x1] < nums2[x2]:
                    nums3.append(nums1[x1])
                    x1 += 1
                else:
                    nums3.append(nums2[x2])
                    x2 += 1
            
        
            for w in range(m+n):
                nums1[w] = nums3[w]

나름 필요한 경우만 딱딱 모아서 깔끔하게 썼다고 생각했는데 runtime이 상위 40%정도밖에 안된다(...)

이 문제는 나중에 한 번 더 풀어보는걸로!