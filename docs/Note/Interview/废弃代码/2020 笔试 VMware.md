
```py
import sys


class VMWARE2020:
    # 一个不含前导0的非负整数，如果从左往右看和从右往左看，它都是一样的，就称其为回文数，例如12321，123321，8，88，707，1920291均为回文数，而29，887，96，9990，8088均不是回文数。现要求你输出所有n位回文数中的第k小。
    def q1(self, n, k):
        if n == 1:
            return k-1
        if n % 2 == 0:
            tmp = 10**(n//2-1)
            tmp += k-1
            res = str(tmp)+str(tmp)[::-1]
        else:
            tmp = 10**(n//2)
            tmp += k-1
            res = str(tmp)+str(tmp)[-2::-1]
        return res

    # 已知有n个物品，每个物品有两个属性$a_i$和$b_i$，现在要求你把这些物品分为A，B两组，要求满足以下条件：
    # 1. A中物品的数量 <= $\sum^B a_i$
    # 2. B中物品的$\sum b_i$尽量小
    # 问B中的$\sum b_i$最小是多少。
    def q2(self, n, items):
        # [[0, 10], [30, 20], [1, 4], [1, 6]]
        # dp?
        # 背包
        if n < 2:
            return 0 
        items = sorted(items,key=lambda x: x[0])
        # dp[i] 前i个物品放入A中, 最小的的 物品？
        dp = [{} for i i n range(n)]
        for i in range(1,i):
            pass
        
    
    # 给出一个闭区间[a,b]和一个正整数k，请你找出符合如下条件的数x的个数：
    #   1. 数字可以被k整除，即x%k==0;
    #   2. 数字不能被[2,k-1]中的任何一个数整除。（当k-1小于2时，区间可被认为成一个空集，即满足不能被任何一个数整除的条件）
    def q3(self, a, b, k):
        def isNeed(num, k):
            if num % k != 0:
                return False
            for i in range(3, k):
                if num % i == 0:
                    return False
            if num % 2 == 0:
                return False
            return True

        if k-1 < 2:
            return 0
        count = 0
        for i in range(a, b+1):
            if isNeed(i, k):
                count += 1
        return count


def q1_main():
    solution = VMWARE2020()
    T = int(sys.stdin.readline().strip())
    for i in range(T):
        line = list(map(int, sys.stdin.readline().strip().split(' ')))
        n = line[0]
        k = line[1]
        print(solution.q1(n, k))


def q2_main():
    print(VMWARE2020().q2(4, [[0, 10], [30, 20], [1, 4], [1, 6]]))
    # n = int(sys.stdin.readline().strip())
    # items = []
    # for i in range(n):
    #     items.append(list(map(int,sys.stdin.readline().strip().split(' '))))
    # print(VMWARE2020().q2(n,items))


def q3_main():
    line = list(map(int, sys.stdin.readline().strip().split(' ')))
    a = line[0]
    b = line[1]
    k = line[2]
    print(VMWARE2020().q3(a, b, k))


q3_main()
```